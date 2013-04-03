Install 
```bash
lein uberjar
lein install
```

Add to lein deps
```clojure ["simple-date" "1.0.0"]```

# simple-date

```clojure
(ns simple-date.core)

(defn- init-calendar []
  (.. java.util.Calendar getInstance))

(defmacro calendar-type [field]
  (eval
   `(symbol 
     (str "java.util.Calendar/" ~field))))
               
(def day (calendar-type "DAY_OF_MONTH"))
(def month (calendar-type "MONTH"))
(def year (calendar-type "YEAR"))

(defn get-day [date] (.. date (get day)))
(defn get-month [date] (.. date (get month)))
(defn get-year [date] (.. date (get year)))

(defn today [] (init-calendar))

(defn days-ago [n]
  {:pre [(pos? n)]}
  (let [remove (Integer. 
                (str "-" n))
        instance (today)]
    (.. instance
        (add day remove))
    instance))

(defn yesterday [] (days-ago 1))

(defn days-ahead [n]
  {:pre [(pos? n)]}
  (let [instance (today)]
    (.. instance
        (add day n))
    instance))

(defn tomorrow [] (days-ahead 1))

(defn weeks-ago [n]
  {:pre [(pos? n)]}
  (let [remove (->> n
                    (* 7)
                    (str "-")
                    Integer.)
        instance (today)]
    (.. instance
        (add day remove))
    instance))

(defn last-week [] (weeks-ago 1))

(defn weeks-ahead [n]
  {:pre [(pos? n)]}
  (let [addition (* 7 n)
        instance (today)]
    (.. instance
        (add day addition))
    instance))

(defn next-week [] (weeks-ahead 1))

(defn months-ago [n]
  {:pre [(pos? n)]}
  (let [remove (Integer.
                (str "-" n))
        instance (today)]
    (.. instance
        (add month remove))
    instance))

(defn last-month [] (months-ago 1))

(defn months-ahead [n]
  {:pre [(pos? n)]}
  (let [instance (today)]
    (.. instance
        (add month n))
    instance))

(defn next-month [] (months-ahead 1))

(defn years-ago [n]
  {:pre [(pos? n)]}
  (let [remove (Integer.
                (str "-" n))
        instance (today)]
    (.. instance
        (add year remove))
    instance))

(defn last-year [] (years-ago 1))

(defn years-ahead [n]
  {:pre [(pos? n)]}
  (let [instance (today)]
    (.. instance
        (add year n))
    instance))

(defn next-year [] (years-ahead 1))
```

## License

Copyright © 2013 FIXME

Distributed under the Eclipse Public License, the same as Clojure.
