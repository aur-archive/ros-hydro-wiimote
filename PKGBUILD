
pkgdesc="ROS - The wiimote package allows ROS nodes to communicate with a Nintendo Wiimote and its related peripherals, including the Nunchuk, Motion Plus, and (experimentally) the Classic."
url='http://www.ros.org/'

pkgname='ros-hydro-wiimote'
pkgver='1.9.10'
_pkgver_patch=0
arch=('i686' 'x86_64')
pkgrel=1
license=('GPL')

ros_makedepends=(ros-hydro-rospy
  ros-hydro-sensor-msgs
  ros-hydro-genmsg
  ros-hydro-geometry-msgs
  ros-hydro-roslib
  ros-hydro-catkin
  ros-hydro-std-srvs
  ros-hydro-std-msgs)
makedepends=('cmake' 'git' 'ros-build-tools'
  ${ros_makedepends[@]}
  cwiid
  python2-numpy)

ros_depends=(ros-hydro-rospy
  ros-hydro-sensor-msgs
  ros-hydro-genmsg
  ros-hydro-geometry-msgs
  ros-hydro-roslib
  ros-hydro-std-srvs
  ros-hydro-std-msgs)
depends=(${ros_depends[@]}
  cwiid
  python2-numpy)

_tag=release/hydro/wiimote/${pkgver}-${_pkgver_patch}
_dir=wiimote
source=("${_dir}"::"git+https://github.com/ros-gbp/joystick_drivers-release.git"#tag=${_tag})
md5sums=('SKIP')

build() {
  # Use ROS environment variables
  /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/hydro/setup.bash ] && source /opt/ros/hydro/setup.bash

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/hydro \
        -DPYTHON_EXECUTABLE=/usr/bin/python2 \
        -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 \
        -DPYTHON_LIBRARY=/usr/lib/libpython2.7.so \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
