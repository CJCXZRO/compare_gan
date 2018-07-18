## Compare GAN code.

The code was used in "Are GANs Created Equal? A Large-Scale Study"
paper (https://arxiv.org/abs/1711.10337) and in "The GAN Landscape: Losses, Architectures, Regularization, and Normalization" (https://arxiv.org/abs/1807.04720).

If you want to see the version used, only in the first paper - Please see the *v1* branch of this repository.

## Pre-trained models

The pre-trained models are available on TensorFlow Hub. Please see [this colab](https://colab.research.google.com/github/google/compare_gan/blob/master/compare_gan/src/tfhub_models.ipynb)
For an example how to use them.

### Best hyperparameters

The repository contains the values for the best hyperparameters for different combinations of models, regularizations and penalties.
You can see them in ``generate_tasks_lib.py`` file and train using ``--experiment=best_models_sndcgan``

### Installation:

Install, run:

```shell
python -m pip install -e . --user
```

After installing, make sure to run

```shell
compare_gan_prepare_datasets.sh
```

It will download all the necessary datasets and frozen TF graphs. By default it will store them in ``/tmp/datasets``.

WARNING: by default this script only downloads and installs small datasets - it doesn't download celebaHQ or lsun bedrooms.

*  **Lsun bedrooms dataset**: If you want to install lsun-bedrooms you need to run t2t-datagen yourself (this dataset will take couple hours to download and unpack).

*  **CelebaHQ dataset**:  It is not available in tensor2tensor currently. Please use the [ProgressiveGAN github](https://github.com/tkarras/progressive_growing_of_gans) for instructions on how to prepare it.

### Running

compare_gan has two binaries:

  * ``generate_tasks`` - that creates a list of files with parameters to execute.
  * ``run_one_task`` - that executes a given task, both training and evaluation, and stores results in the CSV file.


```shell
# Create tasks for experiment "test" in directory /tmp/results. See "src/generate_tasks_lib.py" to see other possible experiments.
compare_gan_generate_tasks --workdir=/tmp/results --experiment=test

# Run task 0 (training and eval)
compare_gan_run_one_task --workdir=/tmp/results --task_num=0 --dataset_root=/tmp/datasets

# Run task 1 (training and eval)
compare_gan_run_one_task --workdir=/tmp/results --task_num=1 --dataset_root=/tmp/datasets
```

Results (FID and inception scores for checkpoints) will be stored in ``/tmp/results/TASK_NUM/scores.csv``.
