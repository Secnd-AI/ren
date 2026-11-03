# Ren 1.1.0 base

This reference preserves the complete historical snapshot: model research, application code,
integrations, demo and result files. It is not a model-only package or the startup roadmap.

- Code: `Secnd-AI/ren` commit `9599cc928d9a4be058ced9c18c62d0feb11e1468`.
- Reference tag: `ren-1.1.0-base`; protected branch: `base/ren-1.1.0`.
- Weights: Hugging Face `Secnd-AI/ren`, release `v1.1.0`, immutable revision
  `94e90d9b65fe3a18c9c1fe9b574939f84b83f4e6`.
- Calibration threshold selection: `backend.calibrate.choose_threshold` at the code commit above.

Never edit the base. New model and startup work goes on `dev`. Compare against the base on the
same fixed evaluation splits, keeping training, calibration and test data separate. Historical
application code does not define the scope of the next product. This freeze does not remove it.

The weight repository includes three distinct artifacts: the root retrieval encoder, `reader/`
for the decision reader, and `meta/` for the task-tuned checkpoint. Loading the repository root
does not load the decision reader. This example loads the base reader on CPU after installing
the repository's locked Python dependencies; it downloads model files when run:

```python
from pathlib import Path

import torch
from huggingface_hub import snapshot_download
from transformers import AutoTokenizer

from backend.incontext import Reader

revision = "94e90d9b65fe3a18c9c1fe9b574939f84b83f4e6"
root = Path(snapshot_download(
    "Secnd-AI/ren", revision=revision, allow_patterns=["reader/*"]
)) / "reader"
tokenizer = AutoTokenizer.from_pretrained(root / "encoder", local_files_only=True)
model = Reader(str(root / "encoder")).cpu().eval()
model.scorer.load_state_dict(torch.load(
    root / "scorer.pt", map_location="cpu", weights_only=True
))
```

Use the same pinned revision and the `meta/` subdirectory when reproducing the task-tuned
checkpoint. Loading weights does not establish an automatic-answer certificate for a new
workspace. The committed policy files and their documented history scope still apply.
