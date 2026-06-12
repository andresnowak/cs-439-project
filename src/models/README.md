# Models

Model definitions used by the training experiments.

- `gpt.py`: compact GPT architecture with Track-3-style components, including
  q/k/v projections, RMSNorm, RoPE, tied embedding/head weights, and ReLU-squared
  MLPs.

The model shape is controlled through YAML configs, mainly `configs/base.yaml`,
`configs/small.yaml`, and `configs/gpt124m.yaml`.
