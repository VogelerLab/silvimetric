## TreeTally
This software is built with the intion of processing pointcloud data and inserting it into a TileDB data structure as a base, so that future processing can look at this TileDB structure and compute stats bsed on the data present.

### Process

1. A `pdal info` is performed on the data to determine the bounds, or if a polygon is provided then this part makes sure that there are points in the provided polygon
2. The TileDB is created or re-opened
3. The Bounding box is split up into cells and chunks.
4. Using Dask, we then push a chunk to a worker to operate on where we fetch the data with the chunk's bounds, and assign the points in that chunk to the associated cells.
5. This data, now split into cell data (similar to a raster) is then written to TileDB.

### Usage

```
bolepole [-h] [--tdb_dir TDB_DIR] [--threads THREADS] [--workers WORKERS] [--group_size GROUP_SIZE] [--resolution RESOLUTION] [--polygon POLYGON] [--debug DEBUG] [--watch WATCH] filename
```

#### Options
 - `filename`: The path to the pointcloud (String)
 - `--tdb_dir`: The path to the desired TileDB location (String)
   - Defaults to the filename of the pointcloud, or if the pointcloud is EPT, then the directory name
 - `--threads`: Number of threads given to each worker (int)
   - default: 4
 - `--workers`: Number of workers (int)
   - default: 12
 - `--group_size`: Number of cells in a chunk
   - default: 12
 - `--resolution`: Width of a cell
   - default: 30
 - `--debug`: Moves to a single-threaded dask config for debugging purposes
 - `--watch`: Open web browser page that shows current resource usage and task information.
 - `--polygon`: Polygon to filter points by. The cells and chunks will be made using this. This polygon must be in the same SRS as the data.