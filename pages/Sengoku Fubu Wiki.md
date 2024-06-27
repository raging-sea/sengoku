- #+BEGIN_QUOTE
  **Wiki for real-time strategy game Sengoku Fubu.**
  #+END_QUOTE
- ![collabannounce.png](../assets/collabannounce_1719396953296_0.png){:height 437, :width 769}
- **Community** #parallel-2
  background-color:: blue
	- [Discord Server](https://discord.gg/pqXNKw5vrz)
	- [Facebook Page](https://www.facebook.com/sengokufubu.en/)
- What's New #parallel-2
  background-color:: yellow
	- Rashomon S9
	- New system [[Backup Hero]] & [[Teaware]]
- **Events**
  background-color:: green
- query-sort-by:: block
  query-table:: true
  background-color:: #497d46
  query-sort-desc:: false
  query-properties:: [:block]
  #+BEGIN_QUERY
  {:title [:code "📆 This Week"]
          :query
  			[:find (pull ?b [*]) (pull ?block [*]) (pull ?bj [*])
  			:keys btime bparent bjournal
              :in $ ?start ?next
              :where
                  [?b :block/scheduled ?ds] 
  				[?b :block/deadline ?d]
  				[(* 1 ?ds) ?num-ds]
  				[(* 1 ?d) ?num-d]
                  [(<= ?num-ds ?start)]
  				[(>= ?num-d ?next)]
  				[?b :block/parent ?p]
  				[?p :block/parent ?block]
  				[?bj :block/journal? true]
  				[?bj :block/journal-day ?jd]
  				[(= ?num-d ?jd)]	
              ]
  		:inputs [:today :today]
  		:view (fn [rows] 
  			[:table 
  				[:thead 
  					[:tr 
  						[:th "Event"] 
  						[:th "End Date"] 
  						[:th "Category"] 
  					]
  				] 
  				[:tbody 
  					(for [r rows] [:tr 
  						[:td [:a {:href (str "#/page/" (get-in r [:bparent :block/name]))} (str (get-in r [:bparent :block/original-name]))]]
  						[:td [:a {:href (str "#/page/" (get-in r [:bjournal :block/name]))} (str (get-in r [:bjournal :block/original-name]))]]
  						[:td (get-in r [:bparent :block/properties :category])]
  					])
  				]
  			]
  		)		
          :result-transform 
  			(fn [result]
  				(sort-by 
  					(fn [h] (get-in h [:btime :block/deadline]))
  					(sort-by
  						(fn [h] (get-in h [:btime :block/scheduled]))				
  						result)))
          :table-view? true
          :collapsed? false
      }
  #+END_QUERY
- #+BEGIN_QUERY
  {:title [:code "▶️ Upcoming"]
          :query
  			[:find (pull ?b [*]) (pull ?block [*]) (pull ?bj [*]) (pull ?bjj [*])
  			:keys btime bparent bstart bend
              :in $ ?start ?next ?max
              :where
                  [?b :block/scheduled ?ds] 
  				[?b :block/deadline ?d]
                  [(> ?ds ?start)]
  				[(>= ?d ?next)]
  				[(<= ?ds ?max)]
  				[?b :block/parent ?p]
  				[?p :block/parent ?block]
  				[?bj :block/journal? true]
  				[?bj :block/journal-day ?jd]
  				[(= ?ds ?jd)]	
  				[?bjj :block/journal? true]
  				[?bjj :block/journal-day ?jdd]
  				[(= ?d ?jdd)]
              ]
  		:inputs [:today :today :7d-after]
  		:view (fn [rows] 
  			[:table 
  				[:thead 
  					[:tr 
  						[:th "Event"] 
  						[:th "Start"] 
  						[:th "End"]
  						[:th "Category"]
  					]
  				] 
  				[:tbody 
  					(for [r rows] [:tr 
  						[:td [:a {:href (str "#/page/" (get-in r [:bparent :block/name]))} (str (get-in r [:bparent :block/original-name]))]]
  						[:td [:a {:href (str "#/page/" (get-in r [:bstart :block/name]))} (str (get-in r [:bstart :block/original-name]))]]
  						[:td [:a {:href (str "#/page/" (get-in r [:bend :block/name]))} (str (get-in r [:bend :block/original-name]))]]
                        [:td (get-in r [:bparent :block/properties :category])]
  					])
  				]
  			]
  		)		
          :result-transform 
  			(fn [result]
  				(sort-by 
  					(fn [h] (get-in h [:btime :block/scheduled]))
  					(sort-by
  						(fn [h] (get-in h [:btime :block/deadline]))				
  						result)))
          :table-view? true
          :collapsed? false
      }
  #+END_QUERY
- **Update Notes**
  background-color:: yellow
	- 🔥 *NEW* [[Update Notes ver.1.10.11100]]
	- [[Update Notes ver.1.10.11000]]
	- [[Update Notes ver.1.10.10900]]
	- [[Update Notes ver.1.10.10800]]
	- *➜ Find all [[Update Notes]]*
- #+BEGIN_QUERY
  {
   :title [:code "🚧 Known Issue"]
   :query [:find (pull ?b [*])
           :where
           [?b :block/marker ?marker]
           (not (not [?b :block/ref-pages ?p]
           [?p :page/name ?page-name]
           [(clojure.string/includes? ?page-name "known issue")])
           )
           [(contains? #{"TODO" "NOW" "LATER"} ?marker)]]
          :result-transform 
  			(fn [result]
  				(sort-by 
  					(fn [h] (get-in h [:block/deadline]))
  					(sort-by
  						(fn [h] (get-in h [:block/scheduled]))				
  						result)))
          :breadcrumb-show? false
          :collapsed? false
   }
  #+END_QUERY
- #+BEGIN_QUERY
  {
   :title [:code "✔️ Fixed Issue in Last 7 Days"]
   :query [:find (pull ?b [*])
  		 :in $ ?today ?last-week
           :where
           [?b :block/marker ?marker]
           (not (not [?b :block/ref-pages ?p]
           [?p :page/name ?page-name]
           [(clojure.string/includes? ?page-name "known issue")])
           )
           [(contains? #{"DONE"} ?marker)]
  		 [?b :block/content ?c]
  		 [(re-pattern "(?<=--\\[)(\\d{4})-(\\d{2})-(\\d{2})") ?rx]
  		 [(re-find ?rx ?c) [_ ?dy ?dm ?dd]]
  		 [(str ?dy ?dm ?dd) ?yyyymmdd]
  		 [(* 1 ?yyyymmdd) ?done-day]
  		 [(<= ?done-day ?today)]
  		 [(> ?done-day ?last-week)]
  		 ]
  		:inputs [:today :-7d]
          :result-transform 
  			(fn [result]
  				(sort-by 
  					(fn [h] (get-in h [:block/deadline]))
  					(sort-by
  						(fn [h] (get-in h [:block/scheduled]))				
  						result)))
          :breadcrumb-show? false
          :collapsed? false
   }
  #+END_QUERY