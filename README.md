## onyx-seq

Onyx plugin for reading from a seq.

#### Installation

In your project file:

```clojure
[onyx-seq "0.7.0.0"]
```

```clojure
(:require [onyx.plugin.seq])
```

#### Functions

##### sample-entry

Catalog entry:

```clojure
{:onyx/name :entry-name
 :onyx/plugin :onyx.plugin.seq-input/input
 :onyx/type :input
 :onyx/medium :seq
 :seq/elements-per-segment 2
 :onyx/batch-size batch-size
 :onyx/max-peers 1
 :onyx/doc "Reads segments from seq"}
```

Lifecycle entry:

```clojure
[{:lifecycle/task :in
  :lifecycle/calls :onyx.plugin.seq-input/reader-calls}]
```

##### Example Use - Buffered Line Reader

```clojure
(defn inject-in-reader [event lifecycle]
  (let [rdr (FileReader. (:buffered-reader/filename lifecycle))] 
    {:seq/rdr rdr
     :seq/seq (line-seq (BufferedReader. rdr))}))

(defn close-reader [event lifecycle]
  (.close (:seq/rdr event)))

(def in-calls
  {:lifecycle/before-task-start inject-in-reader
   :lifecycle/after-task-stop close-reader})

;; lifecycles

(def lifecycles
  [{:lifecycle/task :in
    :buffered-reader/filename "resources/lines.txt"
    :lifecycle/calls ::in-calls}
   {:lifecycle/task :in
    :lifecycle/calls :onyx.plugin.seq-input/reader-calls}])
```

#### Attributes

| key                          | type      | description
|------------------------------|-----------|------------
|`:seq/elements-per-segment`   | `integer` | The number of elements to compress into a single segment

#### Contributing

Pull requests into the master branch are welcomed.

#### License

Copyright © 2015 Distributed Masonry LLC

Distributed under the Eclipse Public License, the same as Clojure.
