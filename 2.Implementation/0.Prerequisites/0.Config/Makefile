


MAIN := ./Examples/mono_inertial_euroc.cc
TARGET := $(basename $(notdir $(MAIN)))
ORB := lib/libORB_SLAM3.so


cc_srcs := $(shell find src -name "*.cc")
cc_objs := $(patsubst src/%.cc,objs/%.o,$(cc_srcs))

include_dirs := include $(Eigen_INCLUDE) $(OpenCV_INCLUDE) $(DBoW2_INCLUDE) $(Sophus_INCLUDE) $(g2o_INCLUDE)
linking_libs := ORB_SLAM3 $(OpenCV_LIBS) $(DBoW2_LIBS) $(g2o_LIBS)
library_dirs := lib $(OpenCV_LIB_DIR) $(DBoW2_LIB_DIR) $(g2o_LIB_DIR)


I_options := $(include_dirs:%=-I%)
l_options := $(linking_libs:%=-l%)
L_options := $(library_dirs:%=-L%)
r_options := $(library_dirs:%=-Wl,-rpath=%)


compile_options := -std=c++11 -fPIC -w $(I_options)
linking_options := $(l_options) $(L_options) $(r_options)

# Compile ORB lib
objs/%.o : src/%.cc
	@echo [INFO]: Compile $^ to $@
	@mkdir -p $(dir $@)
	@g++ -c $< -o $@ $(compile_options)

$(ORB) : $(cc_objs)
	@echo [INFO]: Link $^ to $@
	@mkdir -p $(dir $@)
	@g++ -shared $^ -o $@


# Compile example
objs/$(TARGET).o : $(MAIN) 
	@echo [INFO]: Compile $< to $@
	@mkdir -p $(dir $@)
	@g++ -c $< -o $@ $(compile_options)

Examples/$(TARGET) : objs/$(TARGET).o $(ORB)
	@echo [INFO]: Link $^ to $@
	@mkdir -p $(dir $@)
	@g++ $< -o $@ $(linking_options)


run : Examples/$(TARGET)
	@./$< \
		/home/nerf/datav/SLAM/ORB_SLAM3/Vocabulary/ORBvoc.txt \
		/home/nerf/datav/SLAM/ORB_SLAM3/Examples/Monocular-Inertial/EuRoC.yaml \
		/home/nerf/datav/Dataset/EuRoc/MH01 \
		/home/nerf/datav/SLAM/ORB_SLAM3/Examples/Monocular-Inertial/EuRoC_TimeStamps/MH01.txt \
		dataset-MH01_monoi

clean :
	@rm -rf objs Examples/$(TARGET)

debug :
	@echo $(DBoW2_LIBS)

