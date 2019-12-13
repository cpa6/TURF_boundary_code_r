# TURF_boundary_code_r
Help to automate the process of delineating boundaries for TURFS in Mexico and associated governance and management

#Set options
options(scipen=999,stringsAsFactors = FALSE)
#Add packages
install.packages("sp")
install.packages("tidyverse")
install.packages("ggplot")
setwd('C:\Users\ATKIN\OneDrive\Desktop\MPA\Data')
read.csv("C:\Users\ATKIN\OneDrive\Desktop\MPA\Data\Turf_polygons.csv")

#import TURF csv file

#Create polygon from TURF csv based on provided coordinates
polys <- SpatialPolygons(list(
  Polygons(list(Polygon(matrix(polygon[1, ], ncol=2, byrow=TRUE))), ID[1]),
  Polygons(list(Polygon(matrix(polygon[2, ], ncol=2, byrow=TRUE))), ID[2])
))
#Create Spatial Data Frame
polys.df <- SpatialPolygonsDataFrame(polys, data.frame(id=ID, row.names=ID))
#Give coordinate reference system
pp <- p
crs(pp) <- NA
crs(pp)
## CRS arguments: NA
crs(pp) <- CRS("+proj=longlat +datum=WGS84")
crs(pp)


#Loop to create multiple polygons 
# Example data
poly.turf <- t(replicate(50, {
  o <- runif(2)
  c(o, o + c(0, 0.1), o + 0.1, o + c(0.1, 0), o)
}))
ID <- paste0('poly', seq_len(nrow(square)))

# Create SP
polys <- SpatialPolygons(mapply(function(poly, id) {
  xy <- matrix(poly, ncol=2, byrow=TRUE)
  Polygons(list(Polygon(xy)), ID=id)
}, split(poly.turf, row(poly.turf)), ID))

# Create SPDF
polys.df <- SpatialPolygonsDataFrame(polys, data.frame(id=ID, row.names=ID))

plot(polys.df, col=rainbow(50, alpha=0.5))


#Add spatial attibutes to polygons
