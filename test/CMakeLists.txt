itk_module_test()

if (ITK_USE_GPU)

  ADD_DEFINITIONS(-DGPU)

  if(GPU_HANDLES_DOUBLE)
     add_definitions(-DWITH_DOUBLE)
  endif()

  if(NVIDIA_GPU)
      add_definitions(-DNVIDIA)
      MESSAGE(STATUS "NVIDIA platform detected")
  endif()

  INCLUDE_DIRECTORIES("${${itk-module}_SOURCE_DIR}/include")
  INCLUDE_DIRECTORIES("${${itk-module}KernelDir}")

  SET(${itk-module}Tests
    itkCPURecursiveYvvGaussianImageFilterTest.cxx
    itkGPURecursiveYvvGaussianImageFilterTest.cxx
    itkYvvGpuCpuSimilarityTest.cxx
    itkYvvBenchmark.cxx
    itkYvvWhiteImageTest.cxx
    )
else()
    #No GPU, but CPU user will probably want double-precision.
    add_definitions(-DWITH_DOUBLE)
    SET(${itk-module}Tests
    itkCPURecursiveYvvGaussianImageFilterTest.cxx
    itkYvvBenchmark.cxx
    itkYvvWhiteImageTest.cxx
    )
endif()

#set(ITK_TEST_DRIVER itkTestDriver)

CreateTestDriver(${itk-module} "${${itk-module}-Test_LIBRARIES}" "${${itk-module}Tests}")

#Common tests for CPU and/or GPU
itk_add_test(NAME itkCPURecursiveYvvGaussianImageFilterTest2D
  COMMAND ${itk-module}TestDriver
      itkCPURecursiveYvvGaussianImageFilterTest
      DATA{Input/512ex.jpg} 2 12.0 4)

itk_add_test(NAME itkYvvBenchmark2D
    COMMAND ${itk-module}TestDriver
    itkYvvBenchmark
    DATA{Input/512ex.jpg} 2 12.0 4)

itk_add_test(NAME itkYvvBenchmark3D
    COMMAND ${itk-module}TestDriver
    itkYvvBenchmark
    DATA{Input/256x256x64.tif} 3 12.0 2)

itk_add_test(NAME itkYvvWhiteImageTest2D
    COMMAND ${itk-module}TestDriver
      itkYvvWhiteImageTest
      2 12.0 6 512 512)

itk_add_test(NAME itkYvvWhiteImageTest3D
    COMMAND ${itk-module}TestDriver
      itkYvvWhiteImageTest
      3 12.0 2 256 256 16)

if (ITK_USE_GPU)
    itk_add_test(NAME itkGPURecursiveYvvGaussianImageFilterTest2DLow
        COMMAND ${itk-module}TestDriver
        itkGPURecursiveYvvGaussianImageFilterTest
        DATA{Input/512ex.jpg} 2 0.5 1)
        
    itk_add_test(NAME itkGPURecursiveYvvGaussianImageFilterTest2D
        COMMAND ${itk-module}TestDriver
        itkGPURecursiveYvvGaussianImageFilterTest
        DATA{Input/512ex.jpg} 2 20 1)

    itk_add_test(NAME itkYvvGpuCpuSimilarityTest2D
        COMMAND ${itk-module}TestDriver
        itkYvvGpuCpuSimilarityTest
        DATA{Input/512ex.jpg} 2 14)
endif()
