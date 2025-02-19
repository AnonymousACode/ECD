# ECD
The implementation of Equivariant Causal Diffusion.

## Environment Setup

### Environment
The required packages for ECD can be installed using the following command:

```
pip install -r requirements.txt
```

### QM9 dataset

The dataset of QM9 will be automatically downloaded when running training.

### GEOM -Drugs dataset

1. Download the file at https://dataverse.harvard.edu/file.xhtml?fileId=4360331&version=2.0   (Warning: 50gb):
     `wget https://dataverse.harvard.edu/api/access/datafile/4360331`
   
2. Untar it and move it to data/geom/
  `tar -xzvf 4360331`
3. `pip install msgpack`
4. `python3 build_geom_dataset.py`

## Training

### QM9

Training the EDM:

```python main_qm9.py --n_epochs 3000 --exp_name edm_qm9 --n_stability_samples 1000 --diffusion_noise_schedule polynomial_2 --diffusion_noise_precision 1e-5 --diffusion_steps 1000 --diffusion_loss_type l2 --batch_size 64 --nf 512 --n_layers 9 --lr 1e-4 --normalize_factors [1,4,10] --test_epochs 20 --ema_decay 0.9999```


After training

To analyze the sample quality of molecules

```python eval_analyze.py --model_path outputs/edm_qm9 --n_samples 10_000```

To visualize some molecules

```python eval_sample.py --model_path outputs/edm_qm9 --n_samples 10_000```


### GEOM-Drugs

First follow the intructions at data/geom/README.md to set up the data.

Training
```python main_geom_drugs.py --n_epochs 3000 --exp_name edm_geom_drugs --n_stability_samples 500 --diffusion_noise_schedule polynomial_2 --diffusion_steps 1000 --diffusion_noise_precision 1e-5 --diffusion_loss_type l2 --batch_size 64 --nf 256 --n_layers 4 --lr 1e-4 --normalize_factors [1,4,10] --test_epochs 1 --ema_decay 0.9999 --normalization_factor 1 --model egnn_dynamics --visualize_every_batch 10000```


Analyze

```python eval_analyze.py --model_path outputs/edm_geom_drugs --n_samples 10_000```

Sample

```python eval_sample.py --model_path outputs/edm_geom_drugs```
