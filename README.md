[English](README.md) | [한국어](README.ko.md)

# [Ariel-Data-Challenge-2024](https://www.kaggle.com/competitions/ariel-data-challenge-2024/overview)
## Overview
🏆The task of this competition is to extract the atmospheric spectra from each observation, with an estimate of its level of uncertainty.

When an exoplanet—planets orbiting stars other than our Sun— transits its host star in our line of sight, a tiny fraction of starlight (50–200 photons per million) passes through the planet's atmospheric annulus and interacts with its chemistry, clouds, and winds. These faint signals typically range from 50ppm (for Super-Earth like planets) to 200ppm (for Jupiter like planets) in magnitude and are regularly corrupted by the noise of the instrument. A major component of this noise is due to the inevitable vibration of the spacecraft in space, known as ‘jitter noise’(∼200 ppm). Coupled with other sources of correlated and uncorrelated noises, it is proving difficult to achieve the strict technical requirement of the Ariel Payload design.

## Dataset Description
Data on roughly 1,000 exoplanets by observing them while they transit in front of their host stars.

### Metadata Files
adc_info.csv: Contains analog-to-digital (ADC) conversion parameters (gain and offset) for restoring the original dynamic range of the data. Also includes a star column identifying which star was used for that planet's simulation.      

train_labels.csv: Ground truth spectra.      

axis_info.parquet: Axis information for both instruments.      

wavelength.csv: The wavelength grid for each ground truth spectrum in the dataset.     

### Signal Files
The dataset comprises time series imagery from two separate instruments plus calibration data. Ariel contains multiple optical instruments, each specialising in different spectral bands and observing modes. 

FGS1_signal.parquet: 0.60 and 0.80 µm, You can un-flatten the data with numpy.reshape(135000, 32, 32)     

AIRS-CH0_signal.parquet: 1.95 and 3.90 µm, You can un-flatten the data with numpy.reshape(11250, 32, 356)     

### Calibration files
Calibration files record the electronic characteristics of the sensor and serve as "supporting frames" used in image post-processing to image signal to noise ratio.     

dark.parquet: Dark frames are exposures taken with the shutter closed, capturing the thermal noise and bias level of the sensor. These are used to subtract the dark current from science images.     

dead.parquet: Identifies dead or hot pixels on the sensor. Dead pixels do not respond to light, while hot pixels consistently produce high signal levels regardless of incoming light.     

flat.parquet: Flat field frames are created by imaging a uniformly illuminated surface. They are used to correct for variations in pixel-to-pixel sensitivity and optical system irregularities.     

linear_corr.parquet: Information about the linearity correction of the sensor. The response of the pixels in the detector becomes less linear as they fill with electrons, approaching the point of saturation, where the pixel can no longer collect additional electrons and its response to light becomes flat. For an accurate estimate of the signal, the instrument's response as a function of the received charge is calibrated, and the correction is calculated using a polynomial of degree n. This polynomial allows for the conversion of the number of electrons collected/measured by the pixel into the number of electrons that the detector would have generated with a linear response.     

read.parquet: Read noise frames capture the electronic noise introduced during the readout process of the sensor. This noise is present even when no light falls on the detector.

## Domain knowledge
Overview of review paper about [Transits and Occultations](https://opaque-sword-c15.notion.site/Transit-Occeltation-review-paper-overview-213d57682fa24748a886c6df2d5edc04?pvs=4)
