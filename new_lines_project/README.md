# The Cycle-StarNet New Lines Project

One application of the Cycle-StarNet is to utilize the correlations found by the network in order to fill in the missing pieces in our theoretical models. To do this, we train the Cycle-StarNet with a _synthetic domain_ and an _observed domain_. The network will learn correlations between known information (i.e. the absorption lines in our synthetic spectra) and currently unknown information (for instance, spectral lines in our observed spectra for which we do not know the source). Since our synthetic spectra are generated from stellar labels (including chemical abundances) we form a link between these chemical abundances and the unidentified absorption lines. Next, we investigate the correlations that the network has found to extract intuition from the model and possibly identify these missing pieces in our theoretical models.

### Contents

[Mock Dataset](#mock-dataset)

   - [Getting Started](#code)
   

## Mock Dataset

We first test our method with a "mock dataset" that is meant to mimic a real-world application. For this, we will mask absorption lines in our synthetic domain and try to let the network correlate these lines - that are present in our observed domain - to information found in the synthetic spectra. To create a controlled setting, we will generate our observed spectra using The Payne and add noise to the data. The synthetic spectra will also be generated from The Payne, but about 30% of the lines will be set to the continuum level.

### Code
  
  1. Download the synthetic (csn_kurucz.h5) and mock observed ( csn_apogee_mock.h5) training sets from [here](https://www.canfar.net/storage/list/starnet/public/Cycle_SN) and place it in the [data directory](../data/).
  
  2. Next, the line mask is created in [this notebook](./Create_Line_Mask.ipynb).
  
  3. Now we can generate our mock observed dataset [here](./Generate_Observed_Payne_Domain.ipynb).
  
  4. The model architecture and hyper-parameters are set within configuration file in [the config directory](../configs). For instance, I have already created the [new_lines_1 configuration file](../configs/new_lines_1.ini).
  
  5. Using this model as my example, from the main Cycle_SN directory, you can run `python train_network.py new_lines_1 -v 1000 -ct 15` which will train your model displaying the progress every 1000 batch iterations and saves the model every 15 minutes. This same command will continue training the network if you already have the model saved in the [model directory](../models) from previous training. (Note that the training takes approximately 10 hours on GPU). Alternatively, if operating on compute-canada see [this script](../scripts/new_lines_1.sh) for the training. It allows faster data loading throughout training.
  
  6. Lastly, the [Tracking New Lines notebook](./Track_Lines_Mock.ipynb) shows the method used to identify the missing lines.
