- #+BEGIN_QUOTE
  **This is the unofficial Wiki for mobile game Sengoku Fubu.**
  #+END_QUOTE
- ![5 anniversary.jpg](../assets/5_anniversary_1698120289884_0.jpg){:height 294, :width 768}
- **Community**
  background-color:: #264c9b
	- [Discord Server](https://discord.gg/pqXNKw5vrz)
	- [Facebook Page](https://www.facebook.com/sengokufubu.en/)
- **Current Events** #kanban
  background-color:: #497d46
	- query-table:: true
	  query-sort-desc:: false
	  query-properties:: [:block]
	  query-sort-by:: block
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
- **Update Notes**
  background-color:: yellow
	- [[Update Notes ver.1.9.10000]]
	- [[Update Notes ver.1.9.9900]]
	- [[Update Notes ver.1.9.9800]]