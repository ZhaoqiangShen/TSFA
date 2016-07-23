% ****************************************************************
% Tested with Matlab Version R2015a.
% If you get an error please download Matlab 2015a and run this script by
% opening Matlab 2015a.
% Please ensure that Matlab 2015a is installed in another directory as
% opposed to your current version
% If you encounter an error or a bug with Matlab 2015a; please contact us at
% neuroimage.analyst@gmail.com
% ****************************************************************
% This script has been tested only at b-values of 1000 s/mm2 and 30-45
% diffusion weighted directions although different b-values and diffusion
% encoding directions will work. If you encounter an error or a bug with a
% different b-value/diffusion encoding direction; please contact us at
% neuroimage.analyst@gmail.com
% The script currently works with only 1 b0 image (1 non-diffusion weighted
% image).
% Hence, if you have multiple b0-files, please either average the b0
% (preferred) or pick the most suitable b0 (with the highest SNR ;   Not
% recommended) and place that b0 file at the top of the diffusion weighted
% data
% For a single b0 image, one neednot put the b0 map at the top.
% Even if b0 image is not at the top, it will find the location of b0 map
% automatically and will throw an error if more than 1 b0 file is found.
% Before, using this script and averaging b0; please correct for eddy
% current distortions using DTIStudio or eddy in FSL.
% If you have multiple averages (runs) of diffusion weighted images, please
% perform eddy current distortion, average the multiple runs and input the
% averaged raw diffusion data.
% Since, this script is designed for Tract Specific FA, it is recommended
% that fiber tracking has already been performed using DTIStudio,
% ProbtrackX of FSL, TrackVis from MGH group or any other software.
% Please input a "binary" mask of the tract of interest.
% Whole brain mask will work fine but there wont be a "Tract Specific FA"
% map.
% Please enter the RAW files as obtained from the scanner. For Example:
% Philips scanners export the files as .rec, Siemens/GE export as DICOM.
% To stay consistent with all the processing steps, it is expected that the
% orientation information as present in NIFTI files be removed.
% ###################################################################################
% !!! THIS IS A VERY IMPORTANT STEP AS THE SCRIPT WONT ENSURE CORRECT FLIPPING, CORRECT
% ORIENTATIONS etc..IT IS EXPECTED THAT THE USER HAS ALREADY TAKEN CARE OF
% THIS IMPORTANT STEP. IF THE RESULTS DONT MAKE SENSE, PLEASE CHECK IF THIS
% IS ENSURED AND PROPERLY CORRECTED FOR!!!
% ###################################################################################
%
%*******************************************************************
% Please ensure that you unzip the whole zip file sent and put in the path of Matlab
%*******************************************************************
%
%*******************************************************
% Inputs:
% --------------
%  path where the averaged raw, averaged, single b0 diffusion weighted
%  images are located.
% Name of the 4D diffusion weighted data
% Gradient table in the format 3*N where N is the number of diffusion
% encoding directions:
% Eg:
%  0, 0, 0
% 0.4,-0.2,0.8
% ..... upto N rows
% Name of the binary mask of the tract of interest. This should be placed
% in the same location as the raw diffusion weighted dataset.
% b-value
% Dimensions of the 4D files
% Whether all the temporary files are set or not. Please look below the
% recommended usage
% Complete output path
% ***********************************************************************************
% VERY IMPORTANT: If TSFA is desired: please ensure correct flipping of the gradient directions
% ***********************************************************************************
%*************************************************************************
% A dataset is attached and the procedure explained with respect to the attached dataset.
%*************************************************************************
%
%
% ***************************************************************************************************
% Run this script as:
%----------------------------------------
% ***************************************************************************************************
% TSFA(path_of_the_file,name_of_the_4d_file;name_of_the_gradient_table.txt,name_of_the_binary_mask,b_value, dimensions,need_all_output_flag,output_path)
%
%------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
% Example for the test dataset:
% TSFA('C:\Users\Virendra\Desktop\test_subject\Your_directory_should_have_minimum_of_this','average_1_float.rec','Jones30_gradient.txt','CST_Binary_Fiber.dat',1000,[256 256 65],0,'C:\Users\Virendra\Desktop\test_subject\TSFA_Outputs')
%
%------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
% ***************************************************************************************************
%
%
%*******************************************************
% Outputs:
% -------------
%*******************************************************
% There are several ouputs created. If one is only interested in FA map,
% set need_all_output = 1
% Default behavior is only ST FA with and without fiso correction, fiso fraction, tensor fraction and TSFA
% with fiso correction is saved. (fiso ==> Free water isotropic component)
% FA_ST_with_fiso_correction_mask ==> Single Tensor FA with fiso correction
% FA_selected_with_fiso_correction_mask ==> TSFA with fiso correction
% fiso_fit_mask ==> Fraction of free water
% fraction_selected_mask == > Fraction of tract of interest
%*******************************************************
%
%
%
% Developed by Dr. Virendra Mishra as a part of his PhD thesis, Mentored by Dr. Hao Huang
% If you use this work in your research, please cite:
% Mishra, V., Guo, X., Delgado, M. R. and Huang, H. (2015), Toward tract-specific fractional anisotropy (TSFA) at crossing-fiber regions with clinical diffusion MRI. Magn Reson Med, 74: 1768–1779. doi: 10.1002/mrm.25548
% **********************************************************************
% Any comments/suggestions/bug: Please send an email to
% neuroimage.analyst@gmail.com
% **********************************************************************