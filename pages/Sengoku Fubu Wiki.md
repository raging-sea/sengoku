- #+BEGIN_QUOTE
  **This is the unofficial Wiki for real-time strategy game Sengoku Fubu.**
  #+END_QUOTE
- ![5 anniversary.jpg](../assets/5_anniversary_1698120289884_0.jpg){:height 294, :width 768}
- **Community**
  background-color:: blue
	- [Discord Server](https://discord.gg/pqXNKw5vrz)
	- [Facebook Page](https://www.facebook.com/sengokufubu.en/)
- **Events**
  background-color:: green
- query-sort-by:: block
  query-table:: true
  background-color:: #497d46
  query-sort-desc:: false
  query-properties:: [:block]
  #+BEGIN_QUERY
  {:title [:code "This Week"]
          :query
  			[:find (pull ?b [*]) (pull ?block [*]) (pull ?bj [*])
  			:keys btime bparent bjournal
              :in $ ?start ?next
              :where
                  [?b :block/scheduled ?ds] 
  				[?b :block/deadline ?d]
                  [(<= ?ds ?start)]
  				[(>= ?d ?next)]
  				[?b :block/parent ?p]
  				[?p :block/parent ?block]
  				[?bj :block/journal? true]
  				[?bj :block/journal-day ?jd]
  				[(= ?d ?jd)]	
              ]
  		:inputs [:1d-before :today]
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
  {:title [:code "Upcoming"]
          :query
  			[:find (pull ?b [*]) (pull ?block [*]) (pull ?bj [*]) (pull ?bjj [*])
  			:keys btime bparent bstart bend
              :in $ ?start ?next
              :where
                  [?b :block/scheduled ?ds] 
  				[?b :block/deadline ?d]
                  [(> ?ds ?start)]
  				[(>= ?d ?next)]
  				[?b :block/parent ?p]
  				[?p :block/parent ?block]
  				[?bj :block/journal? true]
  				[?bj :block/journal-day ?jd]
  				[(= ?ds ?jd)]	
  				[?bjj :block/journal? true]
  				[?bjj :block/journal-day ?jdd]
  				[(= ?d ?jdd)]
              ]
  		:inputs [:today :today]
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
	- [[Update Notes ver.1.9.10100]]
	- [[Update Notes ver.1.9.10000]]
	- [[Update Notes ver.1.9.9900]]
	- [[Update Notes ver.1.9.9800]]