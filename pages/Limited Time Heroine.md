- #+BEGIN_QUERY
  {:title [:b "Current Limited Heroine"]
   :query [:find (pull ?parent [*])
           :in $ ?start ?today
           :where
           [?b :block/properties ?prop]
           [(get ?prop :edition) ?ed]
           [(= #{"Heroine"} ?ed)]
           [(get ?prop :launch-date) ?date]
           [?j :block/journal? true]
           [?j :block/original-name ?jn]
           [(contains? ?date ?jn)]
           [?j :block/journal-day ?jd]
           [(< ?jd ?today)]
           [(> ?jd ?start)]
  		 [?b :block/parent ?parent]
         ]
   :inputs [:10d-before :30d-after]
   :view (fn [rows] [:table 
   [:thead 
    [:tr 
     [:th "Heroine"] 
     [:th "Launch Date"] ]] 
   [:tbody 
  (for [r rows] [:tr 
     [:td [:a {:href (str "#/page/" (get-in r [:block/name]))} (str (get-in r [:block/original-name]))]]
     [:td (get-in r [:block/properties :launch-date])] ])
     ]]
  )
   :result-transform (fn [result]
                       (sort-by (fn [h]
                                  (get h :block/name)) result))}
  #+END_QUERY