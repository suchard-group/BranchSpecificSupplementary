FROM ubuntu:22.04
WORKDIR /app
ENV LANG=en_US.UTF-8
ENV JAVA_TOOL_OPTIONS=-Dfile.encoding=UTF8
ENV ANT_OPTS=-Dfile.encoding=UTF8
RUN apt-get update && apt-get -y install cmake git gcc g++ openjdk-11-jdk ant
RUN git clone -b hmc-clock --depth 1 https://github.com/beast-dev/beast-mcmc.git && \
cd beast-mcmc && \
git checkout 215bf75e51d465b1f56153836a43c0ebdbd8890e && \
ant
RUN cd /app && \
git clone -b hmc-clock --depth 1 https://github.com/beagle-dev/beagle-lib.git && \
cd beagle-lib && \
git checkout edfb106eb12efae798945ed3dd0a2f918e8c1a28 && \
mkdir build && \
cd build && \
cmake -DBUILD_CUDA=OFF -DBUILD_OPENCL=OFF -DBUILD_OPENMP=OFF -DBUILD_SSE=OFF .. && \
make install && \
apt-get autoremove && apt-get autoclean
ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
WORKDIR /tmp
RUN echo 'alias beast="java -jar ../app/beast-mcmc/build/dist/beast.jar -beagle_CPU -beagle_SSE_off -force_resume"' >> ~/.bashrc
EXPOSE 3000


