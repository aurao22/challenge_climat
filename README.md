# Data description

The input data consists in 7 data sets of surface temperature anomalies all over the world. Each data set is composed of 10 years of data from 22 climate model realizations for 3072 points scattered all over the world. The target data consists of average temperature anomaly values for 192 positions in the next 5 years. Considering the Healpix nested ordering property the downgrading from 3072 points to 192 points is given by the mean of each 16 consecutive values (e.g in python x_down = numpy.mean(x.reshape(192,16),1)).

The two files train_X.csv and test_X.csv with respectively 5 and 2 data sets have the same format with 6 columns:

    `ID` : a unique ID for each value.

    `DATASET`: the dataset id. There are several independent data sets per file. Each dataset is composed of:
        3072 temperature anomalies all over the world from 22 models (model id from 1 to 22) during 10 years (time from 0 to 9)
        3072 temperature anomalies all over the world actually observed (model id = 0) during 10 years (time from 0 to 9)
        the predicted 192 temparature anomalies all over the world for the 22 models (time equals 10)

    `MODEL`: model id (1-22) for models and (0) or the actual observation
    `TIME`: timestamp as integers (0-9) for the 10 year history and 10 for the predicted date.
    `POSITION`: earth coordinates in healpix (nside=4 for prediction, nside=16 for history) the ordering is nested
    `VALUE` : the corresponding temperature anomalies.

The two files train_Y.csv and test_Y contain temperature anomaly observations for the same 5 and 2 data sets as in the X files. They have 5 columns:

    `ID` : a unique ID for each value.
    `DATASET`: the ID of the dataset (0-4) corresponding to the dataset ID of the train_X.csv file;
    `POSITION`: earth coordinates in healpix with nside=4. The ordering is nested.
    `MEAN` : the observed temperature anomaly.
    `VARIANCE` : is left empty since values are observed.

The random_bench.csv file is an example of a candidate file. It should contain the predicted mean temperature values and the corresponding variance for 192 regions for each data set. This file should have the same 5 columns as the Y files:

    `ID` : a unique ID for each value.
    `DATASET`: the ID of the dataset (0-2) corresponding to the dataset ID of the test_X.csv file;
    `POSITION`: earth coordinates in healpix with nside=4. The ordering is nested.
    `MEAN` : the temperature anomaly prediction.
    `VARIANCE`: the variance of the temperature prediction.

Two other python software files should help to understand the challenge:

    climate_example.py provides an example of prediction using the last known value for the mean and the 22 model distribution for the variance.

    show_model.py shows the 10 year map and the corresponding prediction for one model. This python script uses the healpy package (https://healpy.readthedocs.io/en/latest/) to describe the earth coordinates. The script is inside the metric_plot.pdf. NB: Healpy is currently not supported on Windows.
