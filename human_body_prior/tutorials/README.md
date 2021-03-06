# Sampling Novel Body Poses from VPoser
![alt text](../../github_data/latent_interpolation_1.gif "Interpolation of novel poses on the smoother VPoser latent space.")
![alt text](../../github_data/latent_interpolation_2.gif "Interpolation of novel poses on the smoother VPoser latent space.")

You can use VPoser to produce novel random poses. 
This generative model can be potentially used as a synthetic data generator to train other data dependent models.
For this you would need to first obtain a trained VPoser model from https://smpl-x.is.tue.mpg.de/downloads .
Afterwards you can use the following code snippet to sample new poses: 
```python
from human_body_prior.tools.model_loader import load_vposer

expr_dir = 'TRAINED_MODEL_DIRECTORY' # in this directory the trained model along with the model code exist
num_sample_batch = 10 # number of body poses in each batch
 
vposer_pt, ps = load_vposer(expr_dir, vp_model='snapshot')
sampled_pose_body = vposer_pt.sample_poses(num_poses=num_sample_batch) # will a generate Nx1x21x3 tensor of body poses  
```

To visualize the generated poses you can use the *sample_body_pose* script. 
The visualization code requires you to give the path to a SMPL model, obtainable from https://smpl-x.is.tue.mpg.de/downloads
The following shows an example:
```python
from human_body_prior.body_model.body_model import BodyModel
from human_body_prior.tutorials.sample_body_pose import sample_vposer

bm_path = 'PATH_TO_SMPL_model.npz' # obtain from https://smpl-x.is.tue.mpg.de/downloads
bm = BodyModel(bm_path, 'smpl')

# expr_dir: directory for the trained model along with the model code. obtain from https://smpl-x.is.tue.mpg.de/downloads
expr_dir = 'TRAINED_MODEL_DIRECTORY'
sample_vposer(expr_dir, bm, 5, vp_model='snapshot')
```

![alt text](../../github_data/tutorial_sample_body_pose.png "Samples generated by VPoser shown in various angles.")

The each column of the first row of the above image are the novel poses, and the following rows are varied rotations for demonstration purpose
so that you can see the generated body from different angles.

# VPoser Latent Space Interpolation
You can go one stap further and investigate the smoothness of the latent space of VPoser. 
To do this you sample two poses poZ<sub>1</sub> and poZ<sub>2</sub> and then use &alpha; &in; [0.,1.] to change the influence 
of each on the interpolated poZ<sub>inp</sub>, e.g.
 
poZ<sub>inp</sub> = &alpha; * poZ<sub>1</sub> + (1-&alpha;)* poZ<sub>2</sub>.

The header animations show the video of body poses that are result of decoding corresponding varying poZ<sub>inp</sub> values.
