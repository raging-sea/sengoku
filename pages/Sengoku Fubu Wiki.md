- #+BEGIN_QUOTE
  **This is the unofficial Wiki for mobile game Sengoku Fubu.**
  #+END_QUOTE
- ![cover-200812.jpg](../assets/cover-200812_1656919621186_0.jpg){:height 316, :width 600}
- **Community**
  background-color:: #264c9b
	- [Discord Server](https://discord.gg/pqXNKw5vrz)
	- [Facebook Page](https://www.facebook.com/sengokufubu.en/)
- **Current Events** #kanban
  background-color:: #497d46
	- query-table:: true
	  query-sort-desc:: true
	  query-properties:: [:block]
	  #+BEGIN_QUERY
	  {:title [:b "This Week"]
	          :query [
	              :find (pull ?block [*])
	              :in $ ?start ?next
	              :where
	                              [?block :block/scheduled ?ds] 
	  							[?block :block/deadline ?d]
	                              [(<= ?ds ?start)]
	  							[(>= ?d ?next)]
	              ]
	  		:inputs [:today :yesterday]
	          :result-transform 
	  			(fn [result]
	  				(sort-by 
	  					(fn [h] (get-in h [:block/scheduled]))
	  					(sort-by
	  						(fn [h] (get-in h [:block/deadline]))				
	  						result)))
	          :table-view? true
	          :collapsed? false
	      }
	  #+END_QUERY
- **Latest Changes** #kanban
  background-color:: #787f97
	- query-sort-by:: updated-at
	  query-sort-desc:: true
	  query-properties:: [:page :updated-at]
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
	- query-properties:: [:page :created-at]
	  query-sort-by:: created-at
	  query-sort-desc:: true
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