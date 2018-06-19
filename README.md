# tree-count-per-geometry1
tree count per census tract, unique identifier boroct2010 instead of ct2010

--trees per census tract (uncorrected by area)
SELECT dcp_2010censustract.ct2010, dcp_2010censustract.geom_2263, count(dpr_2015tree_census.geom_2263) AS treecnt 
FROM admin.dcp_2010censustract LEFT JOIN ecological.dpr_2015tree_census
ON st_dwithin(dcp_2010censustract.geom_2263, dpr_2015tree_census.geom_2263, 0)
WHERE st_area(dcp_2010censustract.geom_2263) > 0
GROUP BY dcp_2010censustract.ct2010, dcp_2010censustract.geom_2263
ORDER BY dcp_2010censustract.ct2010;

--trees per area
CREATE TABLE test.censustract_trees_area AS
SELECT ct2010, geom_2263, treecnt, st_area(geom_2263) as ct_area, treecnt/st_area(geom_2263) as tree_area FROM 
(SELECT dcp_2010censustract.ct2010, dcp_2010censustract.geom_2263, count(dpr_2015tree_census.geom_2263) AS treecnt 
FROM admin.dcp_2010censustract LEFT JOIN ecological.dpr_2015tree_census
ON st_dwithin(dcp_2010censustract.geom_2263, dpr_2015tree_census.geom_2263, 0)
WHERE st_area(dcp_2010censustract.geom_2263) > 0
GROUP BY dcp_2010censustract.ct2010, dcp_2010censustract.geom_2263) as foo
--ORDER BY dcp_2010censustract.ct2010;
