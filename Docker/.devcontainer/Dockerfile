FROM osrf/ros:humble-desktop-full
LABEL authors="majdkhalife"
ENV DEBIAN_FRONTEND=noninteractive

ARG USERNAME=dev
ARG UID=1000
ARG GID=1000
RUN groupadd -g ${GID} ${USERNAME} && \
    useradd -m -s /bin/bash -u ${UID} -g ${GID} ${USERNAME} && \
    apt-get update && apt-get install -y --no-install-recommends sudo && \
    echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/90-${USERNAME}

WORKDIR /ws

RUN apt-get update && apt-get install -y --no-install-recommends \
    gazebo \
    ros-humble-navigation2 \
    ros-humble-nav2-bringup \
    ros-humble-gazebo-ros-pkgs \
    libopencv-dev libeigen3-dev libgtest-dev libgmock-dev \
    python3-colcon-common-extensions \
    ros-humble-bondcpp ros-humble-test-msgs ros-humble-behaviortree-cpp-v3 \
    libgraphicsmagick++-dev libceres-dev libompl-dev libbullet-dev libxtensor-dev \
    build-essential ccache git cmake \
  && rm -rf /var/lib/apt/lists/*

ENV MAKEFLAGS="-j1"
ENV CMAKE_BUILD_PARALLEL_LEVEL="1"

ENV DISPLAY=host.docker.internal:0
ENV QT_X11_NO_MITSHM=1

RUN echo 'source /opt/ros/humble/setup.bash' >> /etc/bash.bashrc

RUN printf '#!/usr/bin/env bash\nset -euo pipefail\nsource /opt/ros/humble/setup.bash\ncolcon build --symlink-install --event-handlers console_direct+ --cmake-args -DBUILD_TESTING=OFF -DCMAKE_BUILD_TYPE=Release\n' > /usr/local/bin/ros_colcon_build && \
    chmod +x /usr/local/bin/ros_colcon_build

USER ${USERNAME}
