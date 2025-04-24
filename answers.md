select * from nyc_census_blocks limit 1;

ANSWERS
SELECT Sum(popn_asian)
FROM nyc_census_blocks;

SELECT Sum(popn_total) AS population
  FROM nyc_census_blocks
  WHERE boroname = 'Manhattan';

  SELECT boroname,
  100.0 * Sum(popn_black)/Sum(popn_total) AS black_pct
FROM nyc_census_blocks
GROUP BY boroname;

SELECT ST_Area(geom)
  FROM nyc_neighborhoods
  WHERE name = 'New Brighton';

SELECT sum(St_Area(geom)/ 4047)
    FROM nyc_census_blocks
    WHERE boroname = 'The Bronx';

SELECT sum(St_Area(geom)/ 4047)
FROM nyc_neighborhoods
WHERE boroname = 'The Bronx';

SELECT COUNT(*)
FROM nyc_census_blocks
WHERE ST_NumInteriorRings(geom) = 0;

SELECT Sum(ST_Length(geom))/1609.34
  FROM nyc_streets;

SELECT sum(ST_Length(geom))
  FROM nyc_streets
  WHERE name = '5th Ave';

SELECT ST_AsGeoJSON(geom)
FROM nyc_neighborhoods
WHERE name = 'Soho';

SELECT Count(*)
FROM nyc_neighborhoods
WHERE name = 'coney island';

SELECT ST_NumGeometries(geom) AS polygon_count
FROM nyc_neighborhoods
WHERE name = 'Coney Island';

SELECT name, Sum(ST_Length(geom)) AS length
FROM nyc_streets
WHERE name IS NOT NULL
GROUP BY name
ORDER BY length DESC
LIMIT 5;

SELECT name, geom
FROM nyc_streets
WHERE name = 'S Oxford St';


SELECT name, ST_AsText(geom)
FROM nyc_streets
WHERE name = 'S Oxford St';

SELECT name, boroname
FROM nyc_neighborhoods
WHERE ST_Intersects(geom, ST_GeomFromText('LINESTRING(586683 4504814,586725 4504552,586750 4504395)',26918));

SELECT name
FROM nyc_streets
WHERE ST_DWithin(geom, ST_GeomFromText('LINESTRING(586683 4504814,586725 4504552,586750 4504395)', 26918), 0.1);

SELECT Sum(popn_total)
  FROM nyc_census_blocks
  WHERE ST_DWithin(geom, ST_GeomFromText('LINESTRING(586683 4504814,586725 4504552,586750 4504395)', 26918), 50);

SELECT DISTINCT GeometryType(geom) AS geometry_type
FROM nyc_subway_stations;

SELECT count(*)
FROM nyc_neighborhoods
WHERE name = 'East Village';

SELECT s.name, s.routes
FROM nyc_subway_stations AS s
JOIN nyc_neighborhoods AS n
ON ST_Contains(n.geom, s.geom)
WHERE n.name = 'East Village';

SELECT DISTINCT n.name, n.boroname
FROM nyc_subway_stations AS s
JOIN nyc_neighborhoods AS n
ON ST_Contains(n.geom, s.geom)
WHERE strpos(s.routes,'4') > 0;

SELECT Sum(popn_total)
FROM nyc_neighborhoods AS n
JOIN nyc_census_blocks AS c
ON ST_Intersects(n.geom, c.geom)
WHERE n.name = 'Financial District';

SELECT SUM(popn_total) AS total_population
FROM nyc_neighborhoods p
JOIN nyc_census_blocks t 
ON ST_Contains(t.geom, p.geom)
WHERE p.name = 'Financial District';

SELECT
  n.name,
  Sum(c.popn_total) / (ST_Area(n.geom) / 1000000.0) AS popn_per_sqkm
FROM nyc_census_blocks AS c
JOIN nyc_neighborhoods AS n
ON ST_Intersects(c.geom, n.geom)
WHERE n.name IN ('East Village', 'West Village')
GROUP BY n.name, n.geom

