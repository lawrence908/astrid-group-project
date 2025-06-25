# **AstrID** - *Astronomical IDentification*

A machine learning project focused on identifying and classifying astronomical objects using data from various space catalogs. Leveraging deep learning techniques, AstrID aims to enhance our understanding of stars, galaxies, and other celestial phenomena.

## Project Overview

The primary goal of AstrID is to develop a robust system for identifying the location of astronomical objects, primarily stars, in images of space taken by terrestrial and satellite radio-telescopes. This project demonstrates the application of computer vision and deep learning techniques to solve real-world astronomical challenges.

## Key Features

- **Data Retrieval**: Automated fetching of high-resolution images and data from space catalogs like Hipparcos and 2MASS
- **Image Processing**: Advanced processing and visualization of astronomical images with WCS overlays
- **Machine Learning**: Classification of different types of stars and celestial objects using a U-Net model
- **Model Training**: End-to-end training pipeline for deep learning models using high-resolution astronomical images
- **Model Evaluation**: Comprehensive evaluation of trained models on validation and test datasets
- **Prediction System**: Real-time predictions on new astronomical data using trained models

## Technical Approach

### Dataset Creation
Since no pre-compiled dataset met our requirements, we built our own comprehensive dataset from scratch. Using the AstroPy library, we queried both the SkyView and Vizier databases to create a rich dataset:

- **250+ high-resolution images** randomly sampled from across the sky
- **Star catalog data** with precise world coordinates and magnitude information
- **Ground truth masks** indicating exact star locations in each image
- **FITS format storage** combining image data, star catalogs, and pixel masks in a single file

Our dataset structure includes:
- **Primary HDU**: Main image data (2D array of pixel values)
- **Star Catalog HDU**: Binary table containing star information (coordinates, magnitudes)
- **Pixel Mask HDU**: Ground truth mask indicating star positions (2D array where 1 indicates star presence)

An example of this can be seen in the image below, where a random image from SkyView is overlaid with the stars in that region listed in the Vizier catalog. Each blue dot indicates the location of a star, according to the vizier catalog.

![output](https://github.com/user-attachments/assets/ff1cbc73-7de3-4342-a5ae-0800b48c0504)

### Model Architecture
We implemented a **U-Net Convolutional Neural Network** using TensorFlow/Keras, specifically designed for image segmentation tasks:

- **Input**: 1024x1024 pixel astronomical images
- **Output**: Binary mask of star locations
- **Architecture**: U-Net with skip connections for precise localization
- **Training**: 30+ epochs with early stopping to prevent overfitting
- **Optimization**: Adam optimizer with binary cross-entropy loss

### Training Process
Our training pipeline included:
- **Hyperparameter optimization** with systematic logging of all training runs
- **Learning rate tuning** and class weight balancing for optimal performance
- **Early stopping** to prevent overfitting after 10 epochs without improvement
- **GPU acceleration** for significantly faster training times
- **Comprehensive logging** to track model performance across different configurations

Our model did take considerable time to train using a CPU, especially as running 30+ epochs seems to be the optimal performance window. Some of our team members were able to run the training on a GPU, which considerably sped up the training process and is recommended. We took additional steps to optimize the training process, such as using early-stopping, which would stop training if no improvement in the model was observed after 10 consecutive training epochs. Given more time, we would have liked to experiment with other metrics for performance, such as f-1 score.

![image](https://github.com/user-attachments/assets/90c17837-e986-462c-8cb9-232c0000aaeb)

![image](https://github.com/user-attachments/assets/599f839c-a268-4c7a-b93e-2a38009bdcfd)
![image](https://github.com/user-attachments/assets/0242d8ea-fb3a-4a71-af8c-3463bbdbe4e4)

## Results & Achievements

### Model Performance
Our final model was able to predict the location of stars with reasonable accuracy. In the below image, the blue circles indicate the known-location of stars from the Vizier catalog, and the red circles indicate our models' predicted locations of stars. Our model does struggle with the interpretation of photographic anomalies, such as the lens flare visible in the below image.

However, anomalous images were sparse in our dataset, and training on a dataset with more images or even identifying the anomalies in our ground-truth mask may help reduce the false prediction of stars in these locations. Interestingly, our model was able to pick up on stars and star-like objects present in the image, but not listed in the Vizier catalog. This indicates that our model is performing well, and may provide a useful analysis of images from uncataloged regions of space.

![StarOverlay](https://github.com/user-attachments/assets/0ceaa54a-ccb7-4c15-8036-89cc033983ff)
![2024_12_05-093117_chris_gray_r_predictions_overlay](https://github.com/user-attachments/assets/2030079b-88dc-4a76-bbb5-08b270209ac0)

Our team found a substantial improvement in the prediction accuracy when we account for the intensity (vmag value) of a star when generating the ground-truth, or 'mask'. This can be observed in the below image, where the locations of stars indicated in the 'mask' are of varying sizes. Previously, the locations of stars were indicated with a single pixel, which caused our model to have problems identifying larger stars in the image.

![validate3](https://github.com/user-attachments/assets/8e4e5480-506f-4d54-849b-b9fa624ca4c1)
![validate3overlay](https://github.com/user-attachments/assets/5933d098-c27d-403b-8b99-e36c3ac02199)

### Key Insights
1. **Star Intensity Matters**: Incorporating star magnitude (vmag values) in ground truth masks significantly improved prediction accuracy
2. **Model Generalization**: The model successfully identified stars not listed in catalogs, suggesting potential for discovering new astronomical objects
3. **Data Quality Impact**: Training on diverse sky regions (avoiding the galactic plane) improved model robustness
4. **Computational Efficiency**: GPU acceleration reduced training time from hours to minutes

### Visual Results
The model produces detailed visualizations showing:
- **Original astronomical images** from SkyView
- **Ground truth masks** with star locations from Vizier catalog
- **Predicted star locations** overlaid on original images
- **Comparison plots** demonstrating model accuracy

## Technical Stack

- **Python 3.10+** with TensorFlow/Keras for deep learning
- **AstroPy** for astronomical data processing and FITS file handling
- **NumPy & Matplotlib** for numerical computing and visualization
- **CUDA/GPU support** for accelerated training
- **FITS format** for standardized astronomical data storage

## Project Impact

This project demonstrates the successful application of modern deep learning techniques to astronomical image analysis. The ability to automatically identify and locate stars in astronomical images has significant implications for:

- **Automated astronomical surveys** and data processing
- **Discovery of new celestial objects** not yet cataloged
- **Large-scale sky mapping** and star catalog maintenance
- **Educational applications** in astronomy and computer vision

The project showcases the intersection of computer vision, machine learning, and astronomy, providing a foundation for more advanced astronomical object classification systems.

## Future Enhancements

Potential areas for future development include:
- **Black hole candidate identification** (stretch goal)
- **Multi-class classification** of different celestial object types
- **Real-time processing** of telescope data streams
- **Integration with larger astronomical databases**
- **Advanced anomaly detection** for unusual astronomical phenomena
