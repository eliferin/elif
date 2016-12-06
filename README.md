% Demo to use normxcorr2 to find a template (a white onion)
% in a larger image (of a pile of vegetables)
clc;    % Clear the command window.
close all;  % Close all figures (except those of imtool.)
imtool close all;  % Close all imtool figures.
clear;  % Erase all existing variables.
workspace;  % Make sure the workspace panel is showing.
format long g;
format compact;
fontSize = 20;


rgbImage = imread('a.png');
% Get the dimensions of the image.  numberOfColorBands should be = 3.
[rows, columns, numberOfColorBands] = size(rgbImage);
% Display the original color image.
subplot(2, 2, 1);
imshow(rgbImage, []);
axis on;
caption = sprintf('Original Color Image, %d rows by %d columns.', rows, columns);
title(caption, 'FontSize', fontSize);
% Enlarge figure to full screen.
set(gcf, 'units','normalized','outerposition',[0, 0, 1, 1]);

% Let's get our template by extracting a small portion of the original image.

smallSubImage = imcrop(rgbImage, [310 355 370-310 400-355]);
% Get the dimensions of the image.  numberOfColorBands should be = 3.
[rows, columns, numberOfColorBands] = size(smallSubImage);
subplot(2, 2, 2);
imshow(smallSubImage, []);
[m n]=size(smallSubImage)
axis on;
title(caption, 'FontSize', fontSize);
channelToCorrelate = 1;  % Use the red channel.
correlationOutput = normxcorr2(smallSubImage(:,:,1), rgbImage(:,:, channelToCorrelate));
subplot(2, 2, 3);
imshow(correlationOutput, []);
axis on;
% Get the dimensions of the image.  numberOfColorBands should be = 1.
[rows, columns, numberOfColorBands] = size(correlationOutput);
title(caption, 'FontSize', fontSize);
% Find out where the normalized cross correlation image is brightest.
[maxCorrValue, maxIndex] = max(abs(correlationOutput(:)));
[yPeak, xPeak] = ind2sub(size(correlationOutput),maxIndex(1));
% Because cross correlation increases the size of the image, 
% we need to shift back to find out where it would be in the original image.
corr_offset = [(xPeak-size(smallSubImage,2)) (yPeak-size(smallSubImage,1))];

% Plot it over the original image.
subplot(2, 2, 4); % Re-display image in lower right.
imshow(rgbImage);
axis on; % Show tick marks giving pixels
hold on; % Don't allow rectangle to blow away image.
% Calculate the rectangle for the template box.  Rect = [xLeft, yTop, widthInColumns, heightInRows]
boxRect = [310 355 370-310 400-355 ];
% Plot the box over the image.
rectangle('position', boxRect, 'edgecolor', 'g', 'linewidth',2);
% Give a caption above the image.
title('Template Image Found in Original Image', 'FontSize', fontSize);
A= imhist(imread('a.pgm'));
r=[];
figure;
for i=1:1:7;
   str = strcat(int2str(i),'.pgm'); % Yatay olarak resimleri sýralýyor
   eval('B=imread(str);');
   C = imhist(B);
   r(i) = corr2(A,C);
   [d,m] = max(r);
end

%% Deny or Permit
if d>0.5
str = strcat(int2str(m),'.pgm'); % Yatay olarak resimleri sýralýyor
eval('L=imread(str);');
imshow(L)
end
    
