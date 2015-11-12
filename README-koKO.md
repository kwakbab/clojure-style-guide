# The Clojure Style Guide

> 롤 모델이 중요하다. <br/>
> -- Officer Alex J. Murphy / RoboCop

이 클로저 스타일 가이드는 모범 사례를 추천하고 있기 때문에
실제 클로저 개발자들은 다른 클로저 개발자들과도 함께 유지보수가능한 코드를 작성할 수 있다.

여기 작성된 스타일 가이드는 실제 개발 환경에서 사용하고 있는 스타일을 기반으로 작성되었다.
또 잘 사용되지 않는 스타일(그게 아무리 좋다고 할지라도...)을 사용해서 생기는 문제를 개선하려는 사람들에 의해 정리된 내용을 담고있다.

가이드는 관련있는 규칙으로 묶은 여러개의 섹션으로 나뉘어져있다.
규칙을 적고나서는 합리적인 근거들을 덧붙이려 노력했다.
(당연한 것이라고 가정한 것들은 생략하는 경우도 있다.)

모든 규칙은 갑자기 솟아난 것이 아니다;
이것들은 모두 소프트웨어 엔지니어로써의 광대한 내 경력,
클로저커뮤니티의 피드백과 제안, 풍부한 클로저 프로그래밍 리소스 (["Clojure Programming"](http://www.clojurebook.com/)
와 ["The Joy of Clojure"](http://joyofclojure.com/)) 이 뒷받침되어있다.

이 가이드는 계속 진행중이다;
몇 섹션은 없고, 어떤 섹션은 미완성이며, 몇개의 규칙들은 예가 부족하기도 하다.
몇개의 룰은 명확히 설명할 예가 없기도 하다.
언젠가 이런 이슈들은 해결될 것이다. &mdash; 일단 지금은 그렇다는 걸 알고 있으면 된다.

클로저 개발 커뮤니티에서 [라이브러리를 위한 코딩 표준](http://dev.clojure.org/display/community/Library+Coding+Standards)
목록을 정리하는 것도 주목해라.

이 가이드는 [Transmuter](https://github.com/TechnoGate/transmuter)를 통해
PDF나 HTML로 복사해 갈 수 있다.

## Table of Contents

* [소스코드 레이아웃 & 구조](#소스코드 레이아웃 & 구조)
* [문법](#문법)
* [네이밍](#네이밍)
* [컬렉션](#컬렉션)
* [뮤테이션](#뮤테이션)
* [문자열](#문자열)
* [예외](#예외)
* [매크로](#매크로)
* [주석](#주석)
    * [주석어노테이션](#주석어노테이션)
* [그밖에](#그밖에)
* [도구들](#도구들)

## 소스코드 레이아웃 & 구조

> 거의 모든 사람들이 자신의 것을 제외한 모든 코딩 스타일이 번잡하고 가독성이 떨어진다고 확신한다.
> 앞문장에서 "자신의 것을 제외한"을 없앤다면 아마도 맞는 말일지도... <br/>
> -- Jerry Coffin (on indentation)

* <a name="two-spaces"></a>
  탭은 들여쓰기 단위별로 **스페이스** 2칸을 사용하라. hard tab사용 안함
<sup>[[link](#two-spaces)]</sup>

    ```Clojure
    ;; 좋은 예
    (when something
      (something-else))

    ;; 나쁜 예 - 4칸 스페이스
    (when something
        (something-else))
    ```

* <a name="vertically-align-fn-args"></a>
  함수의 인수는 세로로 줄 맞춤 한다.
<sup>[[link](#vertically-align-fn-args)]</sup>

    ```Clojure
    ;; 좋은 예
    (filter even?
            (range 1 10))

    ;; 나쁜 예
    (filter even?
      (range 1 10))
    ```

* <a name="vertically-align-let-and-map"></a>
  let이 바인딩하는 것과 map키워드는 세로로 줄맞춤한다.
<sup>[[link](#vertically-align-let-and-map)]</sup>

    ```Clojure
    ;; 좋은 예
    (let [thing1 "some stuff"
          thing2 "other stuff"]
      {:thing1 thing1
       :thing2 thing2})

    ;; 나쁜 예
    (let [thing1 "some stuff"
      thing2 "other stuff"]
      {:thing1 thing1
      :thing2 thing2})
    ```

* <a name="optional-new-line-after-fn-name"></a>
   `defn`을 쓸때 docstring이 없을 때에는,
   함수 이름과 인수 벡터를 한줄에 써도 좋고, 다음 줄에 나눠서 써도 좋다.
<sup>[[link](#optional-new-line-after-fn-name)]</sup>

    ```Clojure
    ;; 좋은 예
    (defn foo
      [x]
      (bar x))

    ;; 좋은 예
    (defn foo [x]
      (bar x))

    ;; 나쁜 예
    (defn foo
      [x] (bar x))
    ```

* <a name="oneline-short-fn"></a>
  인수 벡터와 본체가 짧은 함수 사이의 개행은 생략할 수도 있다.
<sup>[[link](#oneline-short-fn)]</sup>

    ```Clojure
    ;; 좋은 예
    (defn foo [x]
      (bar x))

    ;; 짧은 본체를 가진 함수의 좋은 예
    (defn foo [x] (bar x))

    ;; 다중 인수 함수의 좋은 예
    (defn foo
      ([x] (bar x))
      ([x y]
        (if (predicate? x)
          (bar x)
          (baz x))))

    ;; 나쁜 예
    (defn foo
      [x] (if (predicate? x)
            (bar x)
            (baz x)))
    ```

* <a name="align-docstring-lines"></a>
  여러줄의 docstrings는 들여쓰기한다.
<sup>[[link](#align-docstring-lines)]</sup>

    ```Clojure
    ;; 좋은 예
    (defn foo
      "Hello there. This is
      a multi-line docstring."
      []
      (bar))

    ;; 나쁜 예
    (defn foo
      "Hello there. This is
    a multi-line docstring."
      []
      (bar))
    ```

* <a name="crlf"></a>*
  Unix 스타일로 줄바꿈 하라.
  (*BSD/Solaris/Linux/OS X 사용자들은 기본으로 설정되어 있다. Windows 사용자에겐 특히 주의가 필요하다.)
<sup>[[link](#crlf)]</sup>

    * 만약 Git을 사용하고 있으면, 다음 설정을 추가함으로써 프로젝트가
    Windows 줄바꿈 형식으로 강제 설정되는 것을 막을 수 있을 것이다.:

    ```
    bash$ git config --global core.autocrlf true
    ```

* <a name="bracket-spacing"></a>
  괄호로 시작되거나(`(`, `{`,`[`), 괄호로 끝나는 (`)`, `}` , `]`) 텍스트가 있으면
  다른 텍스트와 공백으로 분리해라.
  반대로, 괄호 바로 뒤와 바로 앞은 괄호와 텍스트 사이에 공백을 남기지 마라 .
<sup>[[link](#bracket-spacing)]</sup>

    ```Clojure
    ;; 좋은 예
    (foo (bar baz) quux)

    ;; 나쁜 예
    (foo(bar baz)quux)
    (foo ( bar baz ) quux)
    ```

> 사람이 이해하기 쉬운 언어 문법은 세미콜론 암을 유발한다.<br/>
> -- Alan Perlis

* <a name="no-commas-for-seq-literals"></a>
  순서가 있는 콜렉션 리터럴의 엘리먼트 사이에 콤마를 사용하지 마라.
<sup>[[link](#no-commas-for-seq-literals)]</sup>

    ```Clojure
    ;; 좋은 예
    [1 2 3]
    (1 2 3)

    ;; 나쁜 예
    [1, 2, 3]
    (1, 2, 3)
    ```

* <a name="opt-commas-in-map-literals"></a>
  맵 리터럴 사용시 적절한 줄바꿈이나 콤마를 통해 가독성을 높이는데에 신경써라.
<sup>[[link](#opt-commas-in-map-literals)]</sup>

    ```Clojure
    ;; 좋은 예
    {:name "Bruce Wayne" :alter-ego "Batman"}

    ;; 좋은 예, 확실히 더 읽기 좋다.
    {:name "Bruce Wayne"
     :alter-ego "Batman"}

    ;; 좋은 예, 좀 더 간결하다.
    {:name "Bruce Wayne", :alter-ego "Batman"}
    ```

* <a name="gather-trailing-parens"></a>
  마지막에 따라오는 괄호들은 따로 다른 줄에 놓지 않고 한 줄에 함께 써라.
<sup>[[link](#gather-trailing-parens)]</sup>

    ```Clojure
    ;; 좋은 예; 한 줄
    (when something
      (something-else))

    ;; 나쁜 예; 구분된 줄
    (when something
      (something-else)
    )
    ```

* <a name="empty-lines-between-top-level-forms"></a>
  최상위의 구문들 사이에는 빈 줄을 넣어라.
<sup>[[link](#empty-lines-between-top-level-forms)]</sup>

    ```Clojure
    ;; 좋은 예
    (def x ...)

    (defn foo ...)

    ;; 나쁜 예
    (def x ...)
    (defn foo ...)
    ```

    관련있는 `def`들을 붙여쓰는 것은 예외다.

    ```Clojure
    ;; 좋은 예
    (def min-rows 10)
    (def max-rows 20)
    (def min-cols 15)
    (def max-cols 30)
    ```

* <a name="no-blank-lines-within-def-forms"></a>
  함수나 매크로 정의 중간에 빈 줄을 넣지 마라.
  짝을 이루는 구조를 표현할 때는 예외다. (예: `let`,`cond` 사용시)
<sup>[[link](#no-blank-lines-within-def-forms)]</sup>

* <a name="80-character-limits"></a>
  가능하면, 한 줄이 80글자 이상되는 것은 피해라.
<sup>[[link](#80-character-limits)]</sup>

* <a name="no-trailing-whitespace"></a>
  맨뒤의 공백은 피해라.
<sup>[[link](#no-trailing-whitespace)]</sup>

* <a name="one-file-per-namespace"></a>
  한 개의 네임스페이스에는 한 개의 파일을 사용해라.
<sup>[[link](#one-file-per-namespace)]</sup>

* <a name="comprehensive-ns-declaration"></a>
  모든 네임스페이스는 포괄적인 의미가 있는 `ns`구문으로 시작해라.
  `ns`는 `import`,`require`,`refer`,`use` 등으로 구성된다.
<sup>[[link](#comprehensive-ns-declaration)]</sup>

    ```Clojure
    (ns examples.ns
      (:refer-clojure :exclude [next replace remove])
      (:require [clojure.string :as s :refer [blank?]]
                [clojure.set :as set]
                [clojure.java.shell :as sh])
      (:use [clojure xml zip])
      (:import java.util.Date
               java.text.SimpleDateFormat
               [java.util.concurrent Executors
                                     LinkedBlockingQueue]))
    ```

* <a name="prefer-require-over-use"></a>
  ns 매크로 사용시 `:use`보다 `:require :refer :all`사용을 권장한다.
<sup>[[link](#prefer-require-over-use)]</sup>

    ```Clojure
    ;; 좋은 예
    (ns examples.ns
      (:require [clojure.zip :refer :all]))

    ;; 나쁜 예
    (ns examples.ns
      (:use clojure.zip))
    ```

* <a name="no-single-segment-namespaces"></a>
  하나로 구분된 네임스페이스는 피해라.
<sup>[[link](#no-single-segment-namespaces)]</sup>

    ```Clojure
    ;; 좋은 예
    (ns example.ns)

    ;; 나쁜 예
    (ns example)
    ```

* <a name="namespaces-with-5-segments-max"></a>
  너무 긴 네임스페이스는 피해라 (예: 5개 이상으로 이루어진 네임스페이스는 피해라.)
<sup>[[link](#namespaces-with-5-segments-max)]</sup>

* <a name="10-loc-per-fn-limit"></a>
  10줄이 넘는 함수는 피해라.
  이상적으로 모든 함수는 5줄보다 적어야한다.
<sup>[[link](#10-loc-per-fn-limit)]</sup>

* <a name="4-positional-fn-params-limit"></a>
  3개이상의 파라미터와 함께 쓰는 리스트나 4개 연속의 파라미터는 피하라.
<sup>[[link](#4-positional-fn-params-limit)]</sup>

## 문법

* <a name="ns-fns-only-in-repl"></a>
  `require`, `refer`와 같이 네임스페이스를 처리하는 함수의 사용을 피하자.
   REPL 환경 밖에서 전혀 필요하지 않다.
<sup>[[link](#ns-fns-only-in-repl)]</sup>

* <a name="declare"></a>
  이 후의 참조에 대해서 `declare`을 사용하자.
<sup>[[link](#declare)]</sup>

* <a name="higher-order-fns"></a>
  `loop/recur`보다 `map`과 같은 고차함수를 사용하자.
<sup>[[link](#higher-order-fns)]</sup>

* <a name="pre-post-conditions"></a>
  함수 내부에 확인할 조건이 있을 때 pre와 post 함수를 사용하자.
<sup>[[link](#pre-post-conditions)]</sup>

    ```Clojure
    ;; 좋은 예
    (defn foo [x]
      {:pre [(pos? x)]}
      (bar x))

    ;; 나쁜 예
    (defn foo [x]
      (if (pos? x)
        (bar x)
        (throw (IllegalArgumentException "x must be a positive number!")))
    ```

* <a name="dont-def-vars-inside-fns"></a>
  함수 안에 var를 선언하지 말자.
<sup>[[link](#dont-def-vars-inside-fns)]</sup>

    ```Clojure
    ;; 매우 나쁜 예
    (defn foo []
      (def x 5)
      ...)
    ```

* <a name="dont-shadow-clojure-core"></a>
  내부 바인딩으로 `clojure.core`의 이름을 사용하지 말자.
<sup>[[link](#dont-shadow-clojure-core)]</sup>

    ```Clojure
    ;; 나쁜 예 - 내부에 clojure.core/map 사용을 강제한다.
    (defn foo [map]
      ...)
    ```

* <a name="nil-punning"></a>
  시퀀스가 비어있을 때 종료하는 조건에는 `seq`를 사용하자. (이 기술은 종종 *nil punning*이라 불린다.)
<sup>[[link](#nil-punning)]</sup>

    ```Clojure
    ;; 좋은 예
    (defn print-seq [s]
      (when (seq s)
        (prn (first s))
        (recur (rest s))))

    ;; 나쁜 예
    (defn print-seq [s]
      (when-not (empty? s)
        (prn (first s))
        (recur (rest s))))
    ```

* <a name="when-instead-of-single-branch-if"></a>
  `(if ... (do ...)` 대신 `when`을 사용하자.
<sup>[[link](#when-instead-of-single-branch-if)]</sup>

    ```Clojure
    ;; 좋은 예
    (when pred
      (foo)
      (bar))

    ;; 나쁜 예
    (if pred
      (do
        (foo)
        (bar)))
    ```

* <a name="if-let"></a>
  `let` + `if` 대신 `if-let`을 사용하자.
<sup>[[link](#if-let)]</sup>

    ```Clojure
    ;; 좋은 예
    (if-let [result (foo x)]
      (something-with result)
      (something-else))

    ;; 나쁜 예
    (let [result (foo x)]
      (if result
        (something-with result)
        (something-else)))
    ```

* <a name="when-let"></a>
  `let` + `when` 대신 `when-let`을 사용하자.
<sup>[[link](#when-let)]</sup>

    ```Clojure
    ;; 좋은 예
    (when-let [result (foo x)]
      (do-something-with result)
      (do-something-more-with result))

    ;; 나쁜 예
    (let [result (foo x)]
      (when result
        (do-something-with result)
        (do-something-more-with result)))
    ```

* <a name="if-not"></a>
  `(if (not ...) ...)` 대신 `if-not`을 사용하자..
<sup>[[link](#if-not)]</sup>

    ```Clojure
    ;; 좋은 예
    (if-not (pred)
      (foo))

    ;; 나쁜 예
    (if (not pred)
      (foo))
    ```

* <a name="when-not"></a>
  `(when (not ...) ...)` 대신 `when-not`을 사용하자.
<sup>[[link](#when-not)]</sup>

    ```Clojure
    ;; 좋은 예
    (when-not pred
      (foo)
      (bar))

    ;; 나쁜 예
    (when (not pred)
      (foo)
      (bar))
    ```

* <a name="when-not-instead-of-single-branch-if-not"></a>
  `(if-not ... (do ...)` 대신 `when-not`을 사용하자.
<sup>[[link](#when-not-instead-of-single-branch-if-not)]</sup>

    ```Clojure
    ;; 좋은 예
    (when-not pred
      (foo)
      (bar))

    ;; 나쁜 예
    (if-not pred
      (do
        (foo)
        (bar)))
    ```

* <a name="not-equal"></a>
  `(not (= ...))` 대신 `not=`을 사용하자.
<sup>[[link](#not-equal)]</sup>

    ```Clojure
    ;; 좋은 예
    (not= foo bar)

    ;; 나쁜 예
    (not (= foo bar))
    ```

* <a name="multiple-arity-of-gt-and-ls-fns"></a>
  비교할 때, 클로저의 `<`, `>`, 기타 함수 들은 여러 개의 인수를 받을 수 있다는 점을 명심하자.
<sup>[[link](#multiple-arity-of-gt-and-ls-fns)]</sup>

    ```Clojure
    ;; 좋은 예
    (< 5 x 10)

    ;; 나쁜 예
    (and (> x 5) (< x 10))
    ```

* <a name="single-param-fn-literal"></a>
  함수 리터럴 안에서 하나의 파라미터를 받는 경우에는 `%1` 보다 `%`를 사용하자.
<sup>[[link](#single-param-fn-literal)]</sup>

    ```Clojure
    ;; 좋은 예
    #(Math/round %)

    ;; 나쁜 예
    #(Math/round %1)
    ```

* <a name="multiple-params-fn-literal"></a>
  함수 리터럴 안에서 여러 개의 파라미터를 받는 경우에는 `%` 보다 `%1`를 사용하자.
<sup>[[link](#multiple-params-fn-literal)]</sup>

    ```Clojure
    ;; 좋은 예
    #(Math/pow %1 %2)

    ;; 나쁜 예
    #(Math/pow % %2)
    ```

* <a name="no-useless-anonymous-fns"></a>
  불필요하게 익명함수로 감싸지 말자.
<sup>[[link](#no-useless-anonymous-fns)]</sup>

    ```Clojure
    ;; 좋은 예
    (filter even? (range 1 10))

    ;; 나쁜 예
    (filter #(even? %) (range 1 10))
    ```

* <a name="no-multiple-forms-fn-literals"></a>
  함수 바디가 여러개의 구문으로 구성되는 경우 함수 리터럴을 사용하지 말자.
<sup>[[link](#no-multiple-forms-fn-literals)]</sup>

    ```Clojure
    ;; 좋은 예
    (fn [x]
      (println x)
      (* x 2))

    ;; 나쁜 예 ( 명시적으로 do 구문이 필요하다.)
    #(do (println %)
         (* % 2))
    ```

* <a name="complement"></a>
  익명함수의 사용보다 `complement` 함수를 사용하자.
<sup>[[link](#complement)]</sup>

    ```Clojure
    ;; 좋은 예
    (filter (complement some-pred?) coll)

    ;; 나쁜 예
    (filter #(not (some-pred? %)) coll)
    ```

    분리함수의 폼 안에 여집합 속성이 있는 경우 이 규칙은 무시하자 (e.g. `even?` and `odd?`).

* <a name="comp"></a>
  코드가 더 간단해 질 수 있다면 `comp`를 활용한다.
<sup>[[link](#comp)]</sup>

    ```Clojure
    ;; `(:require [clojure.string :as str])`이 되었다고 가정...

    ;; 좋은 예
    (map #(str/capitalize (str/trim %)) ["top " " test "])

    ;; 더 좋은 예
    (map (comp str/capitalize str/trim) ["top " " test "])
    ```

* <a name="partial"></a>
  코드가 더 간단해 질 수 있다면 `partial`을 활용한다.
<sup>[[link](#partial)]</sup>

    ```Clojure
    ;; 좋은 예
    (map #(+ 5 %) (range 1 10))

    ;; (틀림없이) 좋은 예
    (map (partial + 5) (range 1 10))
    ```

* <a name="threading-macros"></a>
  구문이 지나치게 중첩되어 있을 때는 쓰레딩 매크로를 권장한다.
  (`->` (thread-first) 와 `->>`(thread-last))
<sup>[[link](#threading-macros)]</sup>

    ```Clojure
    ;; 좋은 예
    (-> [1 2 3]
        reverse
        (conj 4)
        prn)

    ;; 좋지 않은 예
    (prn (conj (reverse [1 2 3])
               4))

    ;; 좋은 예
    (->> (range 1 10)
         (filter even?)
         (map (partial * 2)))

    ;; 좋지 않은 예
    (map (partial * 2)
         (filter even? (range 1 10)))
    ```

* <a name="dot-dot-macro"></a>
  자바를 사용하면서 체이닝 메서드를 호출할 때 `->`보다 `..`를 권한다.
<sup>[[link](#dot-dot-macro)]</sup>

    ```Clojure
    ;; 좋은 예
    (-> (System/getProperties) (.get "os.name"))

    ;; 더 좋은 예
    (.. System getProperties (get "os.name"))
    ```

* <a name="else-keyword-in-cond"></a>
  `cond`에서 모든 경우의 조건표현식을 처리할 때는 `:else` 키워드를 쓴다.
<sup>[[link](#else-keyword-in-cond)]</sup>

    ```Clojure
    ;; 좋은 예
    (cond
      (< n 0) "negative"
      (> n 0) "positive"
      :else "zero"))

    ;; 나쁜 예
    (cond
      (< n 0) "negative"
      (> n 0) "positive"
      true "zero"))
    ```

* <a name="condp"></a>
  비교하려는 값과 조건이 바뀌지 않을 때는 `cond`보다 `condp`를 써라.
<sup>[[link](#condp)]</sup>

    ```Clojure
    ;; 좋은 예
    (cond
      (= x 10) :ten
      (= x 20) :twenty
      (= x 30) :forty
      :else :dunno)

    ;; 훨씬 좋은 예
    (condp = x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)
    ```

* <a name="case"></a>
  조건표현식이 상수일 때 `cond`나 `condp`보다 `case`를 권장한다.
<sup>[[link](#case)]</sup>

    ```Clojure
    ;; 좋은 예
    (cond
      (= x 10) :ten
      (= x 20) :twenty
      (= x 30) :forty
      :else :dunno)

    ;; 더 좋은 예
    (condp = x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)

    ;; 가장 좋은 예
    (case x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)
    ```

* <a name="shor-forms-in-cond"></a>
  `cond`와 관련된 곳에서 짧은 폼을 써라. 불가능하다면 주석이나 빈 줄 등으로 쌍이 잘 구분되도록 힌트를 주자.
<sup>[[link](#shor-forms-in-cond)]</sup>

    ```Clojure
    ;; 좋은 예
    (cond
      (test1) (action1)
      (test2) (action2)
      :else   (default-action))

    ;; 괜찮은 예
    (cond
      ;; test case 1
      (test1)
      (long-function-name-which-requires-a-new-line
        (complicated-sub-form
          (-> 'which-spans
              multiple-lines)))

      (test2)
      (another-very-long-function-name
        (yet-another-sub-form
          (-> 'which-spans
              multiple-lines)))

      :else
      (the-fall-through-default-case
        (which-also-spans 'multiple
                          'lines)))
    ```

* <a name="set-as-predicate"></a>
  참이나 거짓을 판단하는 구문 대신에, `set` 사용할 수 있다면 그렇게 하라.
<sup>[[link](#set-as-predicate)]</sup>

    ```Clojure
    ;; 좋은 예
    (remove #{0} [0 1 2 3 4 5])

    ;; 나쁜 예
    (remove #(= % 0) [0 1 2 3 4 5])

    ;; 좋은 예
    (count (filter #{\a \e \i \o \u} "mary had a little lamb"))

    ;; 나쁜 예
    (count (filter #(or (= % \a)
                        (= % \e)
                        (= % \i)
                        (= % \o)
                        (= % \u))
                   "mary had a little lamb"))
    ```

* <a name="inc-and-dec"></a>
  `(+ x 1)` and `(- x 1)` 대신 `(inc x)` & `(dec x)`를 사용한다.  
<sup>[[link](#inc-and-dec)]</sup>

* <a name="pos-and-neg"></a>
  `(> x 0)`, `(< x 0)` & `(= x 0)` 대신 `(pos? x)`, `(neg? x)` & `(zero? x)`를 사용한다.
<sup>[[link](#pos-and-neg)]</sup>

* <a name="list-star-instead-of-nested-cons"></a>
  중첩된 `cons` 대신 `list*`를 사용한다.
<sup>[[link](#list-star-instead-of-nested-cons)]</sup>

    ```Clojure
    # 좋은 예
    (list* 1 2 3 [4 5])

    # 나쁜 예
    (cons 1 (cons 2 (cons 3 [4 5])))

    ```

* <a name="sugared-java-interop"></a>
  자바를 사용할 때는 줄인 문법을 사용한다.
<sup>[[link](#sugared-java-interop)]</sup>

    ```Clojure
    ;;; 객체 생성
    ;; 좋은 예
    (java.util.ArrayList. 100)

    ;; 나쁜 예
    (new java.util.ArrayList 100)

    ;;; 정적 메서드 사용
    ;; 좋은 예
    (Math/pow 2 10)

    ;; 나쁜 예
    (. Math pow 2 10)

    ;;; 인스턴스 메서드 사용
    ;; 좋은 예
    (.substring "hello" 1 3)

    ;; 나쁜 예
    (. "hello" substring 1 3)

    ;;; 정적 필드 사용
    ;; 좋은 예
    Integer/MAX_VALUE

    ;; 나쁜 예
    (. Integer MAX_VALUE)

    ;;; 인스턴스 필드 사용
    ;; 좋은 예
    (.someField some-object)

    ;; 나쁜 예
    (. some-object someField)
    ```

* <a name="compact-metadata-notation-for-true-flags"></a>
  키워드와 true 값을 가지는 메타데이터는 리더매크로를 사용해서 짧게 줄여 쓴다.
<sup>[[link](#compact-metadata-notation-for-true-flags)]</sup>

    ```Clojure
    ;; 좋은 예
    (def ^:private a 5)

    ;; 나쁜 예
    (def ^{:private true} a 5)
    ```

* <a name="private"></a>
  비공개 코드는 표시를 해라.
<sup>[[link](#private)]</sup>

    ```Clojure
    ;; 좋은 예
    (defn- private-fun [] ...)

    (def ^:private private-var ...)

    ;; 나쁜 예
    (defn private-fun [] ...) ; 비공개가 아님

    (defn ^:private private-fun [] ...) ; 장황하다.

    (def private-var ...) ; 비공개가 아님
    ```

* <a name="attach-metadata-carefully"></a>
  메타데이터를 붙일 때는 어디에 붙일지 주의해서 사용해라.
<sup>[[link](#attach-metadata-carefully)]</sup>

    ```Clojure
    ;; `a`의 var 참조에 메타데이터를 붙인다.
    (def ^:private a {})
    (meta a) ;=> nil
    (meta #'a) ;=> {:private true}

    ;; 빈 해시값에 메타데이터를 붙인다.
    (def a ^:private {})
    (meta a) ;=> {:private true}
    (meta #'a) ;=> nil
    ```

## 네이밍

> 프로그래밍에서 가장 어려운 것은 캐시 무효화하는 것과 네이밍이다.<br/>
> -- Phil Karlton

* <a name="ns-naming-schemas"></a>
  네임스페이스에 대한 네이밍은 다음 두가지 방법 중 하나를 사용하는 것이 좋다:
<sup>[[link](#ns-naming-schemas)]</sup>

    * `project.module`
    * `organization.project.module`

* <a name="lisp-case-ns"></a>
  네임스페이스의 단어 구분은 `lisp-case`를 사용한다.(예 `bruce.project-euler`)
<sup>[[link](#lisp-case-ns)]</sup>

* <a name="lisp-case"></a>
  함수와 변수는 `lisp-case`를 사용한다.
<sup>[[link](#lisp-case)]</sup>

    ```Clojure
    ;; 좋은 예
    (def some-var ...)
    (defn some-fun ...)

    ;; 나쁜 예
    (def someVar ...)
    (defn somefun ...)
    (def some_fun ...)
    ```

* <a name="CamelCase-for-protocols-records-structs-and-types"></a>
  프로토콜, 레코드, 구조체, 타입에는 `CamelCase`를 사용한다. (HTTP, RFC, XML과 같은 약자는
  대문자를 사용한다.)
<sup>[[link](#CamelCase-for-protocols-records-structs-and-types)]</sup>

* <a name="pred-with-question-mark"></a>
  참이나 거짓을 리턴하는 함수의 이름 끝에는 물음표를 붙인다.
  (예., `even?`).
<sup>[[link](#pred-with-question-mark)]</sup>

    ```Clojure
    ;; 좋은 예
    (defn palindrome? ...)

    ;; 나쁜 예
    (defn palindrome-p ...) ; Common Lisp 스타일
    (defn is-palindrome ...) ; 자바 스타일
    ```

* <a name="changing-state-fns-with-exclamation-mark"></a>
  STM 트랜젝션에 안전하지 않은 함수나 매크로 이름 뒤에는 느낌표를 붙인다. (예. `reset!`).
<sup>[[link](#changing-state-fns-with-exclamation-mark)]</sup>

* <a name="arrow-instead-of-to"></a>
  변환 함수 이름에 있는 `to` 대신 `->`를 사용한다.
<sup>[[link](#arrow-instead-of-to)]</sup>

    ```Clojure
    ;; 좋은 예
    (defn f->c ...)

    ;; 좋지 않음
    (defn f-to-c ...)
    ```

* <a name="earmuffs-for-dynamic-vars"></a>
  바인딩이 여러번 될 수 있을 때는 (즉, 다이나믹인 경우), 이름 양쪽에 `*`표시를 해준다.
<sup>[[link](#earmuffs-for-dynamic-vars)]</sup>

    ```Clojure
    ;; 좋은 예
    (def ^:dynamic *a* 10)

    ;; 나쁜 예
    (def ^:dynamic a 10)
    ```

* <a name="dont-flag-constants"></a>
  상수 값은 특별한 표시를 하지 않는다; 특별한 표시가 없으면 모든 것이 상수라고 가정한다.
<sup>[[link](#dont-flag-constants)]</sup>

* <a name="underscore-for-unused-bindings"></a>
  사용하지 않는 인수 이름은 `_`로 디스트럭처링을 해준다.
<sup>[[link](#underscore-for-unused-bindings)]</sup>

    ```Clojure
    ;; 좋은 예
    (let [[a b _ c] [1 2 3 4]]
      (println a b c))

    (dotimes [_ 3]
      (println "Hello!"))

    ;; 나쁜 예
    (let [[a b c d] [1 2 3 4]]
      (println a b d))

    (dotimes [i 3]
      (println "Hello!"))
    ```

* <a name="idiomatic-names"></a>
  `pred`와 `coll`과 같은 `clojure.core`에서 사용된 관용어를 사용한다.
<sup>[[link](#idiomatic-names)]</sup>

    * 함수 안에서:
        * `f`, `g`, `h` - 함수 값
        * `n` - 크기 같은 숫자 값
        * `index` - 숫자 인덱스
        * `x`, `y` - 숫자
        * `xs` - 시퀀스
        * `m` - 맵
        * `s` - 문자열 입력
        * `re` - 정규식 표현
        * `coll` - 컬렉션
        * `pred` - 참이나 거짓을 리턴하는 함수
        * `& more` - 가변 인자
        * `xf` - 트랜스듀서에 xform
    * in macros:
        * `expr` - 표현식
        * `body` - 바디
        * `binding` - 매크로 바인딩 벡터

## 컬렉션

> 10개의 데이터 구조를 10개의 함수로 조작하는 것 보다
> 1개의 데이터 구조를 100개의 함수로 조작하는 것이 낫다. </br>
> -- Alan J. Perlis (최초의 튜링상 수상자)

* <a name="avoid-lists"></a>
  꼭 필요한 경우를 제외하고 제너릭(특정 타입을 가지는) 데이터를 가지는 리스트는 사용하지 않는다.
<sup>[[link](#avoid-lists)]</sup>

* <a name="keywords-for-hash-keys"></a>
  해시 키는 키워드를 권장한다.
<sup>[[link](#keywords-for-hash-keys)]</sup>

    ```Clojure
    ;; 좋은 예
    {:name "Bruce" :age 30}

    ;; 나쁜 예
    {"name" "Bruce" "age" 30}
    ```

* <a name="literal-col-syntax"></a>
  가능하면 리터럴 컬렉션 문법을 권장한다. 다만 set을 정의할 때는 컴파일 타임에
  값이 정해져 있는 경우에만 리터럴 문법을 사용한다.
<sup>[[link](#literal-col-syntax)]</sup>

    ```Clojure
    ;; 좋은 예
    [1 2 3]
    #{1 2 3}
    (hash-set (func1) (func2)) ; 런타임에 값이 결정된다.

    ;; 나쁜 예
    (vector 1 2 3)
    (hash-set 1 2 3)
    #{(func1) (func2)} ; (func1)와 (func2)의 값이 같다면 예외가 발생한다.
    ```

* <a name="avoid-index-based-coll-access"></a>
  가능하면 인덱스를 사용하여 컬렉션에 접근하는 것을 피한다.
<sup>[[link](#avoid-index-based-coll-access)]</sup>

* <a name="keywords-as-fn-to-get-map-values"></a>
  가능하면 맵에서 값을 가져올 때 키워드를 함수처럼 사용하는 것을 권장한다.
<sup>[[link](#keywords-as-fn-to-get-map-values)]</sup>

    ```Clojure
    (def m {:name "Bruce" :age 30})

    ;; 좋은 예
    (:name m)

    ;; 필요 이상으로 많은 단어를 사용한다.
    (get m :name)

    ;; 나쁜 예 - NullPointerException이 발생 할 가능성이 있다.
    (m :name)
    ```

* <a name="colls-as-fns"></a>
  컬렉션은 컬렉션의 항목을 가져오는 함수라는 사실을 잊지 말자.
<sup>[[link](#colls-as-fns)]</sup>

    ```Clojure
    ;; 좋은 예
    (filter #{\a \e \o \i \u} "this is a test")

    ;; 나쁜 예 - 공유하기에는 너무 구리다.
    ```

* <a name="keywords-as-fns"></a>
  키워드는 함수로 사용할 수 있다는 사실을 잊지 말자.
<sup>[[link](#keywords-as-fns)]</sup>

    ```Clojure
    ((juxt :a :b) {:a "ala" :b "bala"})
    ```

* <a name="avoid-transient-colls"></a>
  성능상의 이유가 아니면 transient 컬렉션 사용을 피한다.
<sup>[[link](#avoid-transient-colls)]</sup>

* <a name="avoid-java-colls"></a>
  자바 컬렉션 사용을 피한다.
<sup>[[link](#avoid-java-colls)]</sup>

* <a name="avoid-java-arrays"></a>
  자바 배열 사용을 피한다. 단 자바와 상호작용이 필요하거나 기본 타입을 많이 다뤄서 성능에 영향을 주는
  경우는 제외한다.
<sup>[[link](#avoid-java-arrays)]</sup>

## 뮤테이션

### 레퍼런스

* <a name="refs-io-macro"></a>
  트랜젝션 안에서 뜻하지 않는 반복 실행을 피하기 위해서 모든 I/O 호출에 `io!` 매크로를 사용해서 감싸는
  것을 검토한다.
<sup>[[link](#refs-io-macro)]</sup>

* <a name="refs-avoid-ref-set"></a>
  가능하면 `ref-set` 사용을 피한다.
<sup>[[link](#refs-avoid-ref-set)]</sup>

    ```Clojure
    (def r (ref 0))

    ;; 좋은 예
    (dosync (alter r + 5))

    ;; 나쁜 예
    (dosync (ref-set r 5))
    ```

* <a name="refs-small-transactions"></a>
  트랜젝션의 크기(안에 있는 기능의 수)는 작으면 작을 수록 좋다.
<sup>[[link](#refs-small-transactions)]</sup>

* <a name="refs-avoid-short-long-transactions-with-same-ref"></a>
  같은 레퍼러스를 사용하는 긴 트랜젝션과 짧은 트랜젝션을 함께 사용하는 것을 피한다.
<sup>[[link](#refs-avoid-short-long-transactions-with-same-ref)]</sup>

### 에이전트

* <a name="agents-send"></a>
  CPU bound와 블럭되지 않는 I/O, 다른 쓰레드에만 `send`를 사용한다.
<sup>[[link](#agents-send)]</sup>

* <a name="agents-send-off"></a>
  블럭이 될지 모르거나 Sleep되거나 스레드가 오랫동안 작업할 것 같은 곳에는 `send-off`를 사용한다.
<sup>[[link](#agents-send-off)]</sup>

### 애텀

* <a name="atoms-no-update-within-transactions"></a>
  STM 트랜젝션 안에서 애텀이 변경 되는 것을 피한다.
<sup>[[link](#atoms-no-update-within-transactions)]</sup>

* <a name="atoms-prefer-swap-over-reset"></a>
  가능하면 `reset!`보다 `swap!`을 사용한다.
<sup>[[link](#atoms-prefer-swap-over-reset)]</sup>

    ```Clojure
    (def a (atom 0))

    ;; 좋은 예
    (swap! a + 5)

    ;; 그리 좋지 않음
    (reset! a 5)
    ```

## 문자열

* <a name="prefer-clojure-string-over-interop"></a>
  문자열을 다루는 함수들은 자바에 있는 함수나 스스로 만드는 것보다는
  `clojure.string`의 함수를 쓰는 것이 좋다.
<sup>[[link](#prefer-clojure-string-over-interop)]</sup>

    ```Clojure
    ;; 좋은 예
    (clojure.string/upper-case "bruce")

    ;; 나쁜 예
    (.toUpperCase "bruce")
    ```

## 예외

* <a name="reuse-existing-exception-types"></a>
  가능하면 자바에 있는 예외 타입을 사용하라. 기본 타입(예. `java.lang.IllegalArgumentException`,
  `java.lang.UnsupportedOperationException`,
  `java.lang.IllegalStateException`, `java.io.IOException`)의 예외를 던지는 것이
  일반적인 클로저 코드이다.
<sup>[[link](#reuse-existing-exception-types)]</sup>

* <a name="prefer-with-open-over-finally"></a>
  `finally`보다는 `with-open`을 권장한다.
<sup>[[link](#prefer-with-open-over-finally)]</sup>

## 매크로

* <a name="dont-write-macro-if-fn-will-do"></a>
  함수로 가능한 것을 매크로로 만들지 마라.
<sup>[[link](#dont-write-macro-if-fn-will-do)]</sup>

* <a name="write-macro-usage-before-writing-the-macro"></a>
  매크로 다음에 매크로를 생성하는 예제를 만들어라.
<sup>[[link](#write-macro-usage-before-writing-the-macro)]</sup>

* <a name="break-complicated-macros"></a>
  복잡한 매크로는 가능하다면 좀더 작은 함수들로 분리하라.
<sup>[[link](#break-complicated-macros)]</sup>

* <a name="macros-as-syntactic-sugar"></a>
  매크로는 깔끔한 문법을 제공해야 하고 구현이 의존성 없는 함수로 이루어져아한다.
  그래야 재사용성을 향상시킬 수 있다.
<sup>[[link](#macros-as-syntactic-sugar)]</sup>

* <a name="syntax-quoted-forms"></a>
  그냥 매크로를 작성하는 것 보다 Syntax-quoted 구문을 사용을 권장한다.
<sup>[[link](#syntax-quoted-forms)]</sup>

## 주석

> 좋은 코드는 그 자체가 좋은 문서이다. 주석을 달기 전에 스스로에게 물어봐라, "어떻게 하면 이 주석이 필요없게
> 코드를 개선할 수 있을까?" 코드를 개선해서 문서처럼 보이게 하는 것이다. <br/>
> -- Steve McConnell

* <a name="self-documenting-code"></a>
  가능하면 코드가 스스로 문서화가 될 수 있도록 노력한다.
<sup>[[link](#self-documenting-code)]</sup>

* <a name="four-semicolons-for-heading-comments"></a>
  제목을 나타내는 주석은 최소한 4개의 세미콜론을 사용하여 작성한다.
<sup>[[link](#four-semicolons-for-heading-comments)]</sup>

* <a name="three-semicolons-for-top-level-comments"></a>
  소개를 나타내는 주석은 3개의 세미콜론을 사용하여 작성한다.
<sup>[[link](#three-semicolons-for-top-level-comments)]</sup>

* <a name="two-semicolons-for-code-fragment"></a>
  코드의 특정 부분을 설명하는 주석은 그 코드와 들여쓰기를 맞추고 세미콜론 2개를 사용하여 작성한다.
<sup>[[link](#two-semicolons-for-code-fragment)]</sup>

* <a name="one-semicolon-for-margin-comments"></a>
  코드의 끝에 오는 주석은 세미콜론 하나를 사용해서 작성한다.
<sup>[[link](#one-semicolon-for-margin-comments)]</sup>

* <a name="semicolon-space"></a>
  세미콜론과 주석 내용은 항상 최소 한개의 공백을 두고 작성한다.
<sup>[[link](#semicolon-space)]</sup>

    ```Clojure
    ;;;; Frob Grovel

    ;;; This section of code has some important implications:
    ;;;   1. Foo.
    ;;;   2. Bar.
    ;;;   3. Baz.

    (defn fnord [zarquon]
      ;; If zob, then veeblefitz.
      (quux zot
            mumble             ; Zibblefrotz.
            frotz))
    ```

* <a name="english-syntax"></a>
  한 단어 이상의 주석은 대문자로 시작하고 마침표를 사용한다. 문장의 구분은
  [공백하나](http://en.wikipedia.org/wiki/Sentence_spacing)를 사용한다.
<sup>[[link](#english-syntax)]</sup>

* <a name="no-superfluous-comments"></a>
  쓸데없는 주석은 피한다.
<sup>[[link](#no-superfluous-comments)]</sup>

    ```Clojure
    ;; 나쁜 예
    (inc counter) ; increments counter by one
    ```

* <a name="comment-upkeep"></a>
  주석은 항상 최신 상태로 유지한다. 업데이트 되지 않은 주석은 없는 게 좋다.
<sup>[[link](#comment-upkeep)]</sup>

* <a name="dash-underscore-reader-macro"></a>
  구문을 유지해야하는 경우에는 주석 보다는 `#_` 리더 매크로 사용을 권장한다.
<sup>[[link](#dash-underscore-reader-macro)]</sup>

    ```Clojure
    ;; 좋은 예
    (+ foo #_(bar x) delta)

    ;; 나쁜 예
    (+ foo
       ;; (bar x)
       delta)
    ```

> 좋은 코드는 좋은 농담과 같다 - 설명이 필요없다. <br/>
> -- Russ Olsen

* <a name="refactor-dont-comment"></a>
  나쁜 코드에 대해 설명하는 주석은 피한다. 스스로 문서화가 될 수 있도록 코드를 리팩토링한다.
  (하거나 안하거나 둘 중 하나다. - '시험삼아 한다'는 것은 없다. -- Yoda)
<sup>[[link](#refactor-dont-comment)]</sup>

### 주석어노테이션

* <a name="annotate-above"></a>
  어노테이션은 관련된 코드 바로 위에 작성한다.
<sup>[[link](#annotate-above)]</sup>

* <a name="annotate-keywords"></a>
  어노테이션 키워드는 콜론과 공백 다음에 설명을 작성한다.
<sup>[[link](#annotate-keywords)]</sup>

* <a name="indent-annotations"></a>
  만약 설명이 여러줄인 경우 다음 줄은 설명의 첫번째와 들여쓰기를 맞춘다.
<sup>[[link](#indent-annotations)]</sup>

* <a name="sing-and-date-annotations"></a>
  주석에 관련된 날짜를 태그형식으로 달면 나중에 쉽게 확인 할 수 있다.
<sup>[[link](#sing-and-date-annotations)]</sup>

    ```Clojure
    (defn some-fun
      []
      ;; FIXME: This has crashed occasionally since v1.2.3. It may
      ;;        be related to the BarBazUtil upgrade. (xz 13-1-31)
      (baz))
    ```

* <a name="rare-eol-annotations"></a>
  설명이 명확하지 않고 중복되는 경우 예외적으로 라인 뒷쪽에 어노테이션을 작성합니다. (규칙은 아님)
<sup>[[link](#rare-eol-annotations)]</sup>

    ```Clojure
    (defn bar
      []
      (sleep 100)) ; OPTIMIZE
    ```

* <a name="todo"></a>
  `TODO`는 구현되지 않은 기능을 나중에 추가해야할 때 사용한다.
<sup>[[link](#todo)]</sup>

* <a name="fixme"></a>
  `FIXME`는 코드가 잘못되어서 고쳐야할 때 사용한다.
<sup>[[link](#fixme)]</sup>

* <a name="optimize"></a>
  `OPTIMIZE`는 코드가 느리거나 비효율적이라서 성능상 문제가 생기는 경우에 사용한다.
<sup>[[link](#optimize)]</sup>

* <a name="hack"></a>
  `HACK`은 코드에 냄새가 나는 것 같아 리팩토링이 필요한 경우 사용한다.
<sup>[[link](#hack)]</sup>

* <a name="review"></a>
  `REVIEW`는 의도한대로 동작하는지 확인해야하는 경우에 사용한다.
  예:
  ```
  `REVIEW: Are we sure this is how the client does X currently?``
  ```
<sup>[[link](#review)]</sup>

* <a name="document-annotations"></a>
  다른 어노테이션 키워들은 필요에 따라 작성하고 `README` 같은 곳에 정리해둔다.
<sup>[[link](#document-annotations)]</sup>

## 그밖에

* <a name="be-functional"></a>
  가능하면 상태변경을 피하고, 함수형 방식을 사용하라.
<sup>[[link](#be-functional)]</sup>

* <a name="be-consistent"></a>
  일관성을 지켜라. 이 가이드라인의 가지고 일관성을 유지하는 것이 좋다.
<sup>[[link](#be-consistent)]</sup>

* <a name="common-sense"></a>
  상식에 맞게 해라.
<sup>[[link](#common-sense)]</sup>

## 도구들

클로저 커뮤니티에서 만들어진 코딩스타일을 위한 몇개의 툴이 있다.

* [Slamhound](https://github.com/technomancy/slamhound)는 기존에 작성된 코드에 필요한 ns를
  자동으로 생성해준다.
* [kibit](https://github.com/jonase/kibit)은 [core.logic](https://github.com/clojure/core.logic)에서
  일반적으로 사용되는 함수나 매크로등의 코드 패턴을 기반으로, 정적 코드 분석을 해주는 툴이다.

# 참여하기

이 가이드는 완성된 가이드가 아니다. 클로저 코딩 스타일에 흥미를 가지고 있는 사람들과 함께 만들고 싶다.
그래서 모든 클로저 커뮤니티에서 잘 사용되면 좋겠다.

이 문서를 고치려면, 티켓을 열거나 풀리퀘스트를 보내라. 미리 ㄱㅅㄱㅅ

프로젝트에 금전적인 기부는 [gittip](https://www.gittip.com/bbatsov)을 통해 할 수 있다.

[![Support via Gittip](https://rawgithub.com/twolfson/gittip-badge/0.2.0/dist/gittip.png)](https://www.gittip.com/bbatsov)

# 라이센스

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
[Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.en_US)에
따라 이용가능하다.

# 공유합시다!

커뮤니티 기반 스타일 가이드는 알려질수록 좋다.
이 가이드를 친구나 동료들에게 트윗하거나 공유하라.
댓글이나 의견, 제안은 가이드를 좀 더 좋게 만들 수 있다.
자네, 더 좋은 가이드를 가지고 싶지 않은가?

화이팅,<br/>
[Bozhidar](https://twitter.com/bbatsov)
