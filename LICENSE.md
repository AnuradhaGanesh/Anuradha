clear
% FOR STANDARD IMAGES
LCC1= imread('E:\rice project\crop\PICS\six_panel_lcc1.jpg');
cform = makecform('srgb2lab');
Transform_LCC1 = applycform(LCC1,cform);
LChannel_LCC1 = Transform_LCC1(:, :, 1);
AChannel_LCC1 = Transform_LCC1(:, :, 2);
BChannel_LCC1 = Transform_LCC1(:, :, 3);
meanLStandard{1} = mean2(LChannel_LCC1);
meanAStandard{1} = mean2(AChannel_LCC1);
meanBStandard{1} = mean2(BChannel_LCC1);

%lcc2
LCC2 = imread('E:\rice project\crop\PICS\six_panel_lcc2.jpg');
cform = makecform('srgb2lab');
Transform_LCC2 = applycform(LCC2,cform);
LChannel_LCC2 = Transform_LCC2(:, :, 1);
AChannel_LCC2 = Transform_LCC2(:, :, 2);
BChannel_LCC2 = Transform_LCC2(:, :, 3);
meanLStandard{2} = mean2(LChannel_LCC2);
meanAStandard{2} = mean2(AChannel_LCC2);
meanBStandard{2} = mean2(BChannel_LCC2);
%lcc3
LCC3 = imread('E:\rice project\crop\PICS\six_panel_lcc3.jpg');
cform = makecform('srgb2lab');
Transform_LCC3 = applycform(LCC3,cform);
LChannel_LCC3 = Transform_LCC3(:, :, 1);
AChannel_LCC3 = Transform_LCC3(:, :, 2);
BChannel_LCC3 = Transform_LCC3(:, :, 3);
meanLStandard{3} = mean2(LChannel_LCC3);
meanAStandard{3} = mean2(AChannel_LCC3);
meanBStandard{3} = mean2(BChannel_LCC3);
%lcc4
LCC4 = imread('E:\rice project\crop\PICS\six_panel_lcc4.jpg');
cform = makecform('srgb2lab');
Transform_LCC4 = applycform(LCC4,cform);
LChannel_LCC4 = Transform_LCC4(:, :, 1);
AChannel_LCC4 = Transform_LCC4(:, :, 2);
BChannel_LCC4 = Transform_LCC4(:, :, 3);
meanLStandard{4} = mean2(LChannel_LCC4);
meanAStandard{4} = mean2(AChannel_LCC4);
meanBStandard{4} = mean2(BChannel_LCC4);

%lcc5
LCC5 = imread('E:\rice project\crop\PICS\six_panel_lcc5.jpg');
cform = makecform('srgb2lab');
Transform_LCC5 = applycform(LCC5,cform);
LChannel_LCC5 = Transform_LCC5(:, :, 1);
AChannel_LCC5 = Transform_LCC5(:, :, 2);
BChannel_LCC5 = Transform_LCC5(:, :, 3);
meanLStandard{5} = mean2(LChannel_LCC5);
meanAStandard{5} = mean2(AChannel_LCC5);
meanBStandard{5} = mean2(BChannel_LCC5);

%lcc6
LCC6 = imread('E:\rice project\crop\PICS\six_panel_lcc6.jpg');
cform = makecform('srgb2lab');
Transform_LCC6 = applycform(LCC6,cform);
LChannel_LCC6= Transform_LCC6(:, :, 1);
AChannel_LCC6 = Transform_LCC6(:, :, 2);
BChannel_LCC6 = Transform_LCC6(:, :, 3);
meanLStandard{6} = mean2(LChannel_LCC6);
meanAStandard{6} = mean2(AChannel_LCC6);
meanBStandard{6} = mean2(BChannel_LCC6);


%FOR TEST IMAGE
I  = imread('E:\rice project\crop\modified code\FINAL OUTPUT\cropped images\new5.png');
subplot(3,3,1);
imshow(I), title('original image');
cform = makecform('srgb2lab');
lab_he = applycform(I,cform);
ab = double(lab_he(:,:,2:3));
nrows = size(ab,1);
ncols = size(ab,2);
ab = reshape(ab,nrows*ncols,2);

nColors = 3;
 %repeat the clustering 3 times to avoid local minima
[cluster_idx, cluster_center] = kmeans(ab,nColors,'distance','sqEuclidean', ...
                                      'Replicates',3);
pixel_labels = reshape(cluster_idx,nrows,ncols);
%imshow(pixel_labels,[]), title('image labeled by cluster index');
segmented_images = cell(1,3);
rgb_label = repmat(pixel_labels,[1 1 3]);

for k = 1:nColors
    color = I;
    color(rgb_label ~= k) = 0;
    segmented_images{k} = color;
end

subplot(3,3,2);
imshow(segmented_images{1}), title('objects in cluster 1');
subplot(3,3,3);
imshow(segmented_images{2}), title('objects in cluster 2');
subplot(3,3,4);
imshow(segmented_images{3}), title('objects in cluster 3');
pause(2)
x = inputdlg('Enter the cluster no. containing the ROI only:');
j = str2double(x);
LeafImage = segmented_images{j};
leaf_mask = LeafImage(:,:,2) > 32;
inleaf_pixels = reshape(LeafImage(leaf_mask(:,:,[1 1 1])), [], 3);
figure(3);
imshow(LeafImage);
Transform_Leaf = applycform(LeafImage,cform);
LChannel_Leaf = Transform_Leaf(:, :, 1);
AChannel_Leaf = Transform_Leaf(:, :, 2);
BChannel_Leaf = Transform_Leaf(:, :, 3);
meanLLeaf = mean(LChannel_Leaf(leaf_mask));
meanALeaf = mean(AChannel_Leaf(leaf_mask));
meanBLeaf = mean(BChannel_Leaf(leaf_mask));
mean = {meanLLeaf,meanALeaf,meanBLeaf};





MIT License

Copyright (c) 2017 AnuradhaGanesh

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
