
# Download & Prepare datasets
```sh
git clone https://github.com/hazeld/EPIC-Skills2018.git

mkdir dataset && cd dataset

# [ChopstickUsing, HandDrawing and SonicDrawing]
# Download https://drive.google.com/file/d/1Ck-Dke5AcKMzKsedJfblRcTjSPBklo4P/view to `dataset`
sudo unzip EPIC-Skills2018_videos.zip  # This didn't work on Linux, but did on Windows. Don't know why
cd EPIC-Skills2018_videos/videos
for dir in `ls`
do
	cd $dir && rm -rf Headmounted && mv Stationary/* . && rm -rf Stationary && cd ../
done
cd ../../
mv EPIC-Skills2018_videos/videos/* .
rm -rf EPIC-Skills2018_videos


# [DoughRolling]
mkdir DoughRolling_zip DoughRolling
# Download all video.zip files in http://kitchen.cs.cmu.edu/main.php?recipefilter=Pizza#table to DoughRolling_zip
cd DoughRolling_zip
for i in S07 S08 S09 S11 S12 S14 S15 S16 S17 S18 S19 S20 S22 S25 S28 S29 S30 S31 S32 S33 S34 S35 S36 S37 S40 S41 S47 S48 S49 S50 S51 S52 S53 S54 S55
do
	# wget http://kitchen.cs.cmu.edu/Main/${i}_Pizza_Video.zip
	sudo unzip ${i}_Pizza_Video.zip -d ${i}_Pizza_Video
	# mv ${i}_Pizza_Video/*_Pizza_7150991* . && rm -rf ${i}_Pizza_Video
done
files=`cat ../../EPIC-Skills2018/dough_rolling_segments.csv | awk -F "," '{ print $1 }' | xargs | sed "s/^[^ ]* //"`
for file in ${files}
do
	mv */*${file}* ../DoughRolling
done
find . -type d -exec rm -r {} +
cd ../


# [Suturing, NeedlePassing and KnotTying]
# Follow and instructions and download dataset from https://cirl.lcsr.jhu.edu/research/hmm/datasets/jigsaws_release/
for file in  Suturing 
do
sudo unzip ${file}.zip && \
	cd ${file} && \
	rm -r !("video") && \  # Remove all files and folders except `video` directory. Do not run this as a sudo user.
	mv video/*_capture2.avi . && rm -rf video && cd ../  # Move all video files from video directory
done
```