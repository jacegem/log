---
date: 2024-12-01
tags:
- json
- edn
- dart
- clojure
- class
categories:
- clojure
- json
title: Json 으로 Dart 클래스 만들기
lastMod: 2025-01-03
---








## require

```clojure
(ns user
  (:require
   [camel-snake-kebab.core :as csk]
   [cheshire.core :as cheshire]
   [clojure.string :as string]))
```



## edn 변환

먼저 edn 으로 변환한다. 키는 케밥 케이스 키워드로 변환한다.

```clojure
(def json "{ \"name\": \"Cheshire\", \"needs\": \"a string\" }")
(cheshire/parse-string json csk/->kebab-case-keyword)
```



## 클래스 맵으로 변환

아래와 같은 형태로 만든다.

```clojure
{:클래스명1 [필드들]
 :클래스명2 [{:type 타입 :name 이름 :list false}]
 ...}
```



기본형태 (아래에서 다시 작성할 예정)

```clojure
(defn class-field-map-basic [acc m base-name part-name]
  (let [class-name (keyword (str base-name "-" part-name))]
    (reduce (fn [acc [key val]]
              (let [type (cond
                           (string? val) :string
                           (int? val) :int
                           (boolean? val) :bool
                           (double? val) :double
                           :else nil)]
                (update acc class-name (fn [v] (conj v {:type type
                                                        :name key})))))
            (assoc acc class-name [])
            m)))
```

acc 에 모든 정보를 담는다.

base-name 에는 기본 이름을, part-name에는 이번 클래스에서 사용할 이름의 뒷부분을 전달한다.



여기에, 배열과 맵을 처리할 수 있도록 추가한다.

배열인 경우에는 안에 있는 요소들을 하나로 합쳐서 새로운 클래스를 생성한다.

```clojure
(defn class-field-map [acc m base-name part-name]
  (let [class-name (keyword (str base-name "-" part-name))]
    (reduce (fn [acc [key val]]
              (cond
                (vector? val)  (cond
                                 (map? (first val)) (let [merged (apply merge {} val)]
                                                      (map-field acc key merged base-name class-name {:list true}))
                                 :else (when-let [type (cond
                                                         (nil? (first val)) :string
                                                         (string? (first val)) :string
                                                         (int? (first val)) :int
                                                         (boolean? (first val)) :bool
                                                         (double? (first val)) :double
                                                         :else nil)]
                                         (update acc class-name (fn [v] (conj v {:list true
                                                                                 :type type
                                                                                 :name key})))))
                (map? val) (map-field acc key val base-name class-name {})
                :else (let [type (cond
                                   (string? val) :string
                                   (int? val) :int
                                   (boolean? val) :bool
                                   (double? val) :double
                                   :else nil)]
                        (update acc class-name (fn [v] (conj v {:type type
                                                                :name key}))))))
            (assoc acc class-name [])
            m)))
```



클래스명이 중복되는 경우에는 뒤에 숫자를 붙여서 생성한다. 1부터 ~ 무한

```clojure
(defn name-with-num [name num]
  (if (= num 0)
    name
    (str name "-" num)))

(defn unique-part-name [acc base-name part-name]
  (let [num (->> (range)
                 (map (fn [n]
                        (let [part-name (name-with-num part-name n)
                              exist     (some #{(keyword (str base-name "-" part-name))} (keys acc))]
                          (if (nil? exist)
                            n
                            nil))))
                 (filter #(some? %))
                 (take 1)
                 first)]
    (name-with-num part-name num)))

(defn- map-field [acc key val base-name class-name opts]
  (let [part-name (unique-part-name acc base-name (name key))]
    (-> acc
        (update class-name (fn [v] (conj v (merge opts
                                                  {:type (str base-name "-" part-name)
                                                   :name key}))))
        (class-field-map val base-name part-name))))
```





여기까지 실행해 보면, 아래처럼 나오는 것을 볼 수 있다.

```clojure
(def json "{ \"name\": \"Cheshire\", \"needs\": \"a string\"}")
(def edn (cheshire/parse-string json csk/->kebab-case-keyword))
(class-field-map {} edn "my" "base")

;=> {:my-base [{:type :string, :name :name} {:type :string, :name :needs}]}
```



이제 클래스로 변경한다.

```clojure
(defn class-string-rows [{:keys [annotation postfix string int bool double]
                    :or   {annotation ""
                           postfix    "?"
                           string     "String"
                           int        "int"
                           bool       "bool"
                           double     "double"}} m]
  (mapcat (fn [[class-name fields]]
            (reduce into []
                    [[(str annotation)
                      (str "class " (csk/->PascalCase (name class-name)) " {")]
                     (->> (mapv (fn [field]
                                  (when-let [field-type (:type field)]
                                    (let [field-name (:name field)
                                          type       (cond
                                                       (= field-type :string) string
                                                       (= field-type :int) int
                                                       (= field-type :bool) bool
                                                       (= field-type :double) double
                                                       :else (csk/->PascalCase field-type))
                                          type       (if (:list field)
                                                       (str "List<" type ">")
                                                       type)
                                          name       (csk/->camelCase (name field-name))]
                                      (str type postfix " " name ";"))))
                                fields)
                          (remove nil?))
                     [(str "}")]]))
          m))
```



여기까지 실행해본다.

```clojure
(def json "{ \"name\": \"Cheshire\", \"needs\": \"a string\"}")
(def edn (cheshire/parse-string json csk/->kebab-case-keyword))
(->> (class-field-map {} edn "my" "base")
     (class-string-rows {}))

;=> ("" "class MyBase {" "String? name;" "String? needs;" "}")
```



## 클래스로 변환

이제 하나의 문자열로 합친다.

``` clojure
(def json "{ \"name\": \"Cheshire\", \"needs\": \"a string\"}")
(def edn (cheshire/parse-string json csk/->kebab-case-keyword))
(->> (class-field-map {} edn "my" "base")
     (class-string-rows {})
   	 (string/join "\n"))

;=> "\nclass MyBase {\nString? name;\nString? needs;\n}"
```



실행하는 함수를 만들고

```clojure
(defn convert-to-class
  ([m] (convert-to-class m {}))
  ([m {:keys [base-name part-name]
       :or   {base-name "my"}
       :as   opts}]
   (->> (class-field-map {} m base-name part-name)
        (class-string-rows opts)
        (string/join "\n"))))
```



다양한 경우로 실행해본다.

```clojure
(def m {:string-type "string"
        :int-type    1
        :bool-type   false
        :double-type 0.2
        :list-string ["l", "s"]
        :list-int    [1 2]
        :list-bool   [true false]
        :list-double [2.1 0.5]
        :list-map    [{:s "s"} {:i 1} {:b true} {:d 3.3}]
        :nest-map    {:say {:to 2}}
        :nest-other  {:say {:to "bob"}}})

(convert-to-class m {:base-name "basic"})

;=> "\nclass Basic {\nList<BasicListMap>? listMap;\ndouble? doubleType;\nString? stringType;\nbool? boolType;\nBasicNestMap? nestMap;\nint? intType;\nList<String>? listString;\nBasicNestOther? nestOther;\nList<bool>? listBool;\nList<int>? listInt;\nList<double>? listDouble;\n}\n\nclass BasicListMap {\nString? s;\nint? i;\nbool? b;\ndouble? d;\n}\n\nclass BasicNestMap {\nBasicSay? say;\n}\n\nclass BasicSay {\nint? to;\n}\n\nclass BasicNestOther {\nBasicSay1? say;\n}\n\nclass BasicSay1 {\nString? to;\n}"
```



아래와 같은 결과물을 얻을 수 있다.

```dart
class Basic {
  List<BasicListMap>? listMap;
  double? doubleType;
  String? stringType;
  bool? boolType;
  BasicNestMap? nestMap;
  int? intType;
  List<String>? listString;
  BasicNestOther? nestOther;
  List<bool>? listBool;
  List<int>? listInt;
  List<double>? listDouble;
}

class BasicListMap {
  String? s;
  int? i;
  bool? b;
  double? d;
}

class BasicNestMap {
  BasicSay? say;
}

class BasicSay {
  int? to;
}

class BasicNestOther {
  BasicSay1? say;
}

class BasicSay1 {
  String? to;
}

```




