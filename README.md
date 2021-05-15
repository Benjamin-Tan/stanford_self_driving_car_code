# Stanford Self Driving Car Code

Stanford Code From Cars That Entered DARPA Grand Challenges

Software Infrastructure for Stanford's Autonomous Vehicles

See http://robots.stanford.edu/papers/junior08.pdf

Originally found on Sourceforge

# Updated, attempt to move from rosbuild to catkin
1. catkinize the workspace, several packages still has yet to be ported.
2. compile the packages with custom messages first (velodyne, driving_common, perception, applanix, localize, controller)

``` catkin_make -DCATKIN_WHITELIST_PACKAGES="<package name>" ```

3. compile the rest

``` catkin_make -DCATKIN_WHITELIST_PACKAGES="" ```