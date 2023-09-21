- Query, Latest Updated Pages
  collapsed:: true
	- ```
	  #+BEGIN_QUERY
	  {:title [:b "Latest Updated Pages"]
	    :query [:find (pull ?p [*])
	    :in $ ?end ?daysback
	    :where 
	      [?b :block/page ?p]
	      [?p :block/updated-at ?v]
	      [(* ?daysback 60 60 24 1000) ?range]
	      [(- ?end ?range) ?period]
	      [(>= ?v ?period)]
	      [(< ?v ?end)]
	     ]
	    :inputs [:end-of-today-ms 3]
	   :collapsed? false
	   :table-view? true
	  }
	  #+END_QUERY
	  ```
- Query, Latest Created Pages
  collapsed:: true
	- ```
	  #+BEGIN_QUERY
	  {:title [:b "Latest Added Pages"]
	    :query [:find (pull ?p [*])
	    :in $ ?end ?daysback
	    :where 
	      [?b :block/page ?p]
	      [?p :block/created-at ?v]
	      [(* ?daysback 60 60 24 1000) ?range]
	      [(- ?end ?range) ?period]
	      [(>= ?v ?period)]
	      [(< ?v ?end)]
	     ]
	    :inputs [:end-of-today-ms 3]
	      :result-transform (fn [result]
	                       (sort-by (fn [h]
	                                  (get h :block/created-at)) result))
	   :collapsed? false
	   :table-view? true
	  }
	  #+END_QUERY
	  ```