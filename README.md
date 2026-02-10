# BDNF-Modulated Spiking Network
A computational neuroscience project investigating how Brain-Derived Neurotrophic Factor (BDNF) levels modulate spike-timing-dependent plasticity (STDP) in spiking neural networks for shape recognition.

## Overview
This project implements a spiking neural network using the Brian2 simulator to study the effects of different BDNF concentrations on learning and classification performance. The network learns to classify geometric shapes (circles, squares, triangles) through biologically-inspired STDP learning rules that are modulated by BDNF levels.

## Key Features
- **BDNF-modulated STDP**: Four different BDNF conditions (low, medium, high, high-homeostatic) with distinct learning rate, LTP/LTD scaling, and normalisation schedules
- **Spiking neural architecture**: Poisson-encoded input layer with Leaky Integrate-and-Fire (LIF) output neurons
- **Lateral inhibition**: Winner-take-all dynamics through competitive inhibition
- **Supervised learning**: Teacher signal guides learning during training phase
- **Comprehensive analysis**: Visualisation of receptive fields, decision margins, contribution maps, and performance metrics

## Network Architecture
- **Input layer**: Poisson neurons encoding pixel intensities (one neuron per pixel)
- **Output layer**: 3 LIF neurons (one per class) with teacher signal injection
- **Synapses**: Fully-connected with STDP plasticity
- **Lateral inhibition**: All-to-all inhibitory connections between output neurons

## BDNF Profiles
| BDNF Level       | Learning Rate Scale | LTP Scale | LTD Scale | Normalisation Frequency |
|------------------|---------------------|-----------|-----------|-------------------------|
| Low              | 0.5×                | 0.8×      | 1.1×      | Every epoch             |
| Medium           | 1.0×                | 1.0×      | 1.0×      | Every 2 epochs          |
| High             | 1.5×                | 1.2×      | 0.9×      | Every 3 epochs          |
| High Homeostatic | 1.3×                | 1.1×      | 0.95×     | Every 3 epochs          |

## Requirements
```bash
pip install brian2 numpy matplotlib imageio pandas
```

## Dataset
The network expects a shape classification dataset with the following structure:
```
archive/
├── training/shapes/train/
│   ├── circle/
│   ├── square/
│   └── triangle/
└── shapes/valid/
    ├── circle/
    ├── square/
    └── triangle/
```

**Note**: Update the paths in the notebooks (`TRAIN_ROOT` and `VAL_ROOT`) to match your dataset location.

## Usage
### Training Individual BDNF Conditions
Each notebook trains and evaluates a specific BDNF condition:
- `testing_LOW.ipynb` - Low BDNF condition
- `testing_MED.ipynb` - Medium BDNF condition  
- `testing_HIGH.ipynb` - High BDNF condition
- `testing_HOMEO.ipynb` - High homeostatic BDNF condition

Run any notebook to:
1. Train the network for 5 epochs
2. Save learnt receptive fields
3. Evaluate on validation set
4. Generate visualisations

### Comparative Analysis
Run `testing_RUN_ALL.ipynb` to:
1. Execute all four BDNF conditions
2. Load all trained weights
3. Generate violin plots comparing margin distributions across conditions
4. Produce summary statistics

## Outputs
Each training run produces:
- **Receptive field images**: Visualisation of learnt synaptic weights (`rf_OUT*.png`)
- **Trained weights**: Compressed numpy archives (`trained_receptive_fields_*.npz`)
- **Performance metrics**: Accuracy, confusion matrices, margin distributions
- **Analysis plots**: 
  - Contribution maps showing pixel-wise importance
  - Hardest/easiest examples
  - Score distributions
  - Comparative violin plots

## Key Hyperparameters
```python
# Simulation
dt = 1 ms
Ton = 300 ms          # Stimulus presentation time
Tisi = 50 ms          # Inter-stimulus interval
epochs = 5

# Network
tau_m = 20 ms         # Membrane time constant
v_thr = 4 mV          # Spike threshold
max_rate = 10 Hz      # Maximum input firing rate

# STDP
taupre = 10 ms        # Pre-synaptic trace decay
taupost = 10 ms       # Post-synaptic trace decay
wmax = 0.05           # Maximum synaptic weight
teacher_amp = 6 mV    # Teacher signal amplitude
```

## Results Interpretation
The notebooks provide multiple analysis tools:
- **Confusion matrices**: Per-class accuracy breakdown
- **Top-1 margins**: Confidence measure (difference between top and runner-up predictions)
- **Contribution maps**: Heatmaps showing which pixels influence each class decision
- **Cosine similarity**: Angle-based similarity between input and learnt receptive fields

## Research Questions
This implementation allows investigation of:
1. How BDNF levels affect learning speed and final accuracy
2. Whether homeostatic mechanisms improve robustness
3. The relationship between BDNF, weight normalisation, and generalisation
4. Decision confidence across different neuromodulatory states

## Citation
If you use this code in your research, please cite accordingly.

## Author
Duaa Naveed

## Acknowledgements
Built using the [Brian2](https://brian2.readthedocs.io/) spiking neural network simulator.
