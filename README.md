# tree-count-per-geometry1
tree count per census tract, unique identifier boroct2010 instead of ct2010

--trees per census tract (uncorrected by area)

create table test.gd_tree_perboroct2010_un as
SELECT dcp_2010censustract.boroct2010, dcp_2010censustract.geom_2263, count(dpr_2015tree_census.geom_2263) AS treecnt 
FROM admin.dcp_2010censustract LEFT JOIN ecological.dpr_2015tree_census
ON st_dwithin(dcp_2010censustract.geom_2263, dpr_2015tree_census.geom_2263, 0)
WHERE st_area(dcp_2010censustract.geom_2263) > 0
GROUP BY dcp_2010censustract.boroct2010, dcp_2010censustract.geom_2263
ORDER BY dcp_2010censustract.boroct2010;

--trees per area

CREATE TABLE test.gd_censustract_trees_area AS
SELECT boroct2010, geom_2263, treecnt, st_area(geom_2263) as ct_area, treecnt/st_area(geom_2263) as tree_area FROM 
test.gd_tree_perboroct2010_un as foo;

--ORDER BY dcp_2010censustract.ct2010;
