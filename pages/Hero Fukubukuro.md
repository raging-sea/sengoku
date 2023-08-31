query-sort-by:: block
query-table:: true
query-sort-desc:: false
#+BEGIN_QUERY
{:title [:b "Heroes in Fukubukuro"]
 :query [:find (pull ?parent [*])
         :in $ ?start ?today
         :where
         [?b :block/properties ?prop]
         [(get ?prop :fukubukuro) ?date]
         [?j :block/journal? true]
         [?j :block/original-name ?jn]
         [(contains? ?date ?jn)]
         [?j :block/journal-day ?jd]
         [(< ?jd ?today)]
         [(> ?jd ?start)]
		 [?b :block/parent ?parent]
       ]
 :inputs [:11d-before :7d-after]
 :view (fn [rows] [:table 
 [:thead 
  [:tr 
   [:th "Unique Heroes"] 
   [:th "Start Date"] ] ] 
 [:tbody 
(for [r rows] [:tr 
   [:td [:a {:href (str "#/page/" (get-in r [:block/name]))} (str (get-in r [:block/original-name]))]]
   [:td (get-in r [:block/properties :fukubukuro])] ])
   ]]
)
 :result-transform (fn [result]
                     (sort-by (fn [h]
                                (get h :block/name)) result))}
#+END_QUERY

- Open Hero Fukubukuro to obtain one Unique Hero or SSR Hero from the Hero pool!
- Notes
- Each Fukubukuro has its own Hero pool, including 3 Unique Heroes and 7 common SSR Heroes. The rate of obatining each Hero by opening the Fukubukuro is the same.
- Owning the same name SSR Hero is required for obtaining a Unique Hero. Unique Hero will be transferred to the same name common SSR Hero if you drew a Unique Hero but not owned the same name common SSR Hero.
- Each Hero in Hero pool will only appear once by opening the Hero Fukubukuro.
- Hero will be transferred into awakening item of corresponding region if you draw a Hero you already owned.
- Each kind of Hero Fukubukuro can be only purchased for 3 times.