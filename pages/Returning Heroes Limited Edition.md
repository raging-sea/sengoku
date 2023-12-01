category:: [[Recruitment]]

- #+BEGIN_QUERY
  {:title [:b "Current Returning Heroes"]
   :query [:find (pull ?parent [*])
           :in $ ?start ?today
           :where
           [?b :block/properties ?prop]
           [(get ?prop :returning) ?date]
           [?j :block/journal? true]
           [?j :block/original-name ?jn]
           [(contains? ?date ?jn)]
           [?j :block/journal-day ?jd]
           [(< ?jd ?today)]
           [(> ?jd ?start)]
  		 [?b :block/parent ?parent]
         ]
   :inputs [:25d-before :30d-after]
   :view (fn [rows] [:table 
   [:thead 
    [:tr 
     [:th "Limited Heroes"] 
     [:th "Start Date"] ]] 
   [:tbody 
  (for [r rows] [:tr 
     [:td [:a {:href (str "#/page/" (get-in r [:block/name]))} (str (get-in r [:block/original-name]))]]
     [:td (get-in r [:block/properties :returning])] ])
     ]]
  )
   :result-transform (fn [result]
                       (sort-by (fn [h]
                                  (get h :block/name)) result))}
  #+END_QUERY
- Schedule
  collapsed:: true
	- [[Maeda Matsu, Christmas 2021]], [[Maeda Toshiie, Christmas 2021]]
	  DEADLINE: <2023-12-14 Thu>
	  SCHEDULED: <2023-11-25 Sat>
	- [[Maeda Matsu, Christmas 2021]], [[Maeda Toshiie, Christmas 2021]] 
	  SCHEDULED: <2022-06-30 Thu>
	  DEADLINE: <2022-07-19 Tue>