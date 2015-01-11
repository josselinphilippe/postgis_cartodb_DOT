#PostGIS
#DOT CityRacks are set up, in part to discourage bicycles from being locked to sidewark infrastructure

#DOT resources

#DOT parking regulations data source
http://www.nyc.gov/html/dot/html/about/datafeeds.shtml#parking

#Online form to suggest location for new CityRack:
http://www.nyc.gov/html/dot/html/bicyclists/cityrack-suggest.shtml

#Permit to build on-property bicycle rack:
http://www.nyc.gov/html/dot/downloads/pdf/self-install-rackapp.pdf

#Cityracks gov portal and data source
http://www.nyc.gov/html/dot/html/bicyclists/bicycleparking.shtml

#From DOT:

"DOT's CityRacks provide free sidewalk bicycle parking racks throughout the five boroughs. CityRacks are a convenience for the entire cycling community. Also, the availability of CityRacks parking discourages cyclists from parking at mailboxes, parking meters, trees, and other sidewalk structures."

#COMPARE INTRA-AGENCY DATA AND RESOURCES ALLOCATION/COST DISTRIBUTION

#DATASET_1: DOT bicycle rack locations
#DATASET_2: DOT parking regulation signs

#Both datasets serve as proxies in the analysis of spatial problems related to DOT VISION ZERO goals for equitable transportation infrastructure distribution
#the DOT_parking_signs layer implies agency costs related to:
    -sign production/purchase
    -sign installation
    -sign maintenace
    -sign removal
    
    additionally and more implicitely, costs also include:
    -occupation of sidewalk space (e.g. who benefits from a no-parking sign? And what could replace a no-parking signpost?)
    -general costs related to traffic management operations(e.g. how much does it cost to coordinate modification of parking regulations for temporary construction sites or parades, for instance) 

#the DOT_bicycle_racks_locations layer serves as a proxy for agency investment in bicycling infrastructure


#PostGIS analysis

I-ratio of signs to racks:
II-average distance between racks:
III-average distance between signs (signs at same location?):
...
IV-average number of bicycle racks per city block
V-average number of parking signs per city block

#test sql
I. Ratio of signs to racks

-clean DOT_parking_signs dataset, filter for multiple signs at same location: 

                        SQL: SELECT * FROM dot_parking_signs WHERE cartodb_id NOT IN (SELECT MIN(cartodb_id) FROM dot_parking_signs GROUP BY the_geom)

                        Result:  Count(*) for this sql is 89452 (the number of locations with multiple signs)
            
# total signs: 381444
# of these, signs that share infrastructure (e.g a sign post): 89452

#delete from dataset to improve analysis accuracy

                        SQL: DELETE FROM dot_parking_signs WHERE cartodb_id NOT IN (SELECT MIN(cartodb_id) FROM dot_parking_signs GROUP BY the_geom)

                        Result: 291992

-calculate the number of locations where public space is dedicated to bicycle parking

                        SQL: SELECT COUNT(*) FROM dot_bicycle_racks

                        Result: 11734

-calculate the number of racks at each bicycle parking location (total_racks column)

                        SQL: SELECT SUM(total_rack) FROM dot_bicycle_racks

                        Result: 17680

-calculate ratios

The DOT manages 381444 parking/traffic signs, distributed accross 291992 diffrent locations of parking/traffic signage throughout the city. This compares to 17680 bicycle racks managed by the DOT, distributed across 11734 locations.
    
                        #DOT parking signs allocation ratio: 
                        average of 1.3 signs per location

                        #DOT bicycle racks/parking efficiency ratio: 
                        average of 1.5 racks per location

                        #DOT ratio of parking sign locations to bicycle rack locations:
                        291992/11734
                        24.88

                        There are approximately 25 times as many spaces of public sidewalk dedicated to motorized vehicle parking (signs) as there are bicycle racks in New York City.

    
II- Average distance between racks 

#postGIS tutorial with CartodDB, amazing resource for understanding measurements as they relate to projection input.
http://academy.cartodb.com/courses/04-sql-postgis/lesson-1.html#thegeomwebmercator

# show distances between all points and one defined point
        
                            SELECT
                          *,
                          ST_Distance(
                              the_geom::geography, 
                              CDB_LatLng(37.7833,-122.4167)::geography
                              ) / 1000 AS dist
                        FROM
                          dot_bicycle_racks

#nearest neighbor search (http://workshops.boundlessgeo.com/postgis-intro/knn.html)(http://workshops.boundlessgeo.com/postgis-intro/spatial_relationships.html)

Index-based KNN
“KNN” stands for “K nearest neighbours”, where “K” is the number of neighbours you are looking for.

KNN is a pure index based nearest neighbour search. By walking up and down the index, the search can find the nearest candidate geometries without using any magical search radius numbers, so the technique is suitable and high performance even for very large tables with highly variable data densities.

#find the nearest 10 bicycle racks to one specific rack identified by name(can be defined by any other column)

                        SELECT
                        dot_bicycle_racks.the_geom_webmercator,
                          dot_bicycle_racks.cartodb_id,
                          dot_bicycle_racks.name
                        FROM

                          dot_bicycle_racks
                        ORDER BY
                          dot_bicycle_racks.the_geom_webmercator <->
                          (SELECT the_geom_webmercator FROM dot_bicycle_racks WHERE name = '1 E 60 ST')
                        LIMIT 10
            


                        The syntax of the index-based KNN query places a special “index-based distance operator” in the ORDER BY clause of the query, in this case “<->”. There are two index-based distance operators,

                        <-> means “distance between box centers”
                        <#> means “distance between box edges”
                

