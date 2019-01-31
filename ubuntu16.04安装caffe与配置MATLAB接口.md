# Matlab R2018b配置caffe接口 （CPU_ONLY）
  1. 安装caffe
     ```
        $ git clone https://github.com/BVLC/caffe.git
     ```
  2. 安装依赖
     ```
     $ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
     $ sudo apt-get install --no-install-recommends libboost-all-dev
     $ sudo apt-get install libatlas-base-dev
     $ sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
     ```
  3. 修改``makefile``和``makefile.config``
     ```
     > makefile.config
     # CPU-only switch (uncomment to build without GPU support).
     CPU_ONLY := 1
     
     # We need to be able to find libpythonX.X.so or .dylib.
     # PYTHON_LIB := /usr/lib
     PYTHON_LIB := $(ANACONDA_HOME)/lib

     # Whatever else you find you need goes here.
     INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial
     LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial/
     # MATLAB directory should contain the mex binary in /bin.
      MATLAB_DIR := /usr/local
      MATLAB_DIR := /usr/local/MATLAB/R2018b
     ```
  4. 添加链接文件库 
     ``fatal error: hdf5.h: No such file or directory``  
      [参考链接](https://blog.csdn.net/greenlight_74110/article/details/78568501)  
      ```
      cd /usr/lib/x86_64-linux-gnu
      sudo ln -s libhdf5_serial.so.8.0.2 libhdf5.so
      sudo ln -s libhdf5_serial_hl.so.8.0.2 libhdf5_hl.so
      ```
      ```
      INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/ and 
      LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib    /usr/lib/x86_64-linux-gnu/hdf5/serial/ 
      ```
  5. 编译caffe
      ```
      $ make all
      $ make test
      $ make runtest
      ```
   6. 配置caffe  MATLAB接口
      ```
      $ make matcaffe
      $ make mat test
      ```
      将caffe/matlab/添加到MATLAB路径
  7.  测试  
      ```
      >> caffe.run_tests()
      Cleared 0 solvers and 0 stand-alone nets
      正在运行 caffe.test.test_net
      .....
      已完成 caffe.test.test_net
      __________

      正在运行 caffe.test.test_solver
      .
      已完成 caffe.test.test_solver
      __________

      正在运行 caffe.test.test_io
      .
      已完成 caffe.test.test_io
      __________

      Cleared 2 solvers and 4 stand-alone nets
      ans = 
      1×7 TestResult 数组 - 属性:

         Name
         Passed
         Failed
         Incomplete
         Duration
         Details
      总计:
         7 Passed, 0 Failed, 0 Incomplete.
         0.75222 秒测试时间。
      ```
  8.  解决类析构之类的报错  
      [参考链接](https://github.com/BVLC/caffe/pull/5642/commits/cea9181d6794cc338857483d1692ed387a0f75a9)  
      ``Net.m``  

      ```
      function delete (self)
         if self.isvalid
            caffe_('delete_net', self.hNet_self);
         end
      end
      ```
      ``Solver.m``  
      ```
      function delete(self)
         if self.isvalid
            caffe_('delete_solver', self.hSolver_self);
         end
      end
      ```
