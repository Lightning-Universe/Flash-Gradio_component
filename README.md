<div align="center">
<img src="https://pl-bolts-doc-images.s3.us-east-2.amazonaws.com/lai.png" width="200px">

A Lightning component to serve a Flash Task using Gradio

______________________________________________________________________

</div>

## Install

Use these instructions to install:

```bash
git clone https://github.com/PyTorchLightning/LAI-flash-gradio.git
cd LAI-flash-gradio
pip install -r requirements.txt
pip install -e .
```

## Use the component

**Note:** This component currently only supports `text_classification` task. So make sure to pass:

```python
run_dict = {
    "task": "text_classification",
    "checkpoint_path": "<path to your checkpoint file>",
}
```

Copy the following code to a file `app.py`, and run the app using: `lightning run app app.py`.

```python
import lightning as L
from lightning import LightningApp

from flash_gradio import FlashGradio


class FlashGradioComponent(L.LightningFlow):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.flash_gradio = FlashGradio()

    def run(self):
        run_dict = {
            "task": "text_classification",
            "checkpoint_path": "checkpoint.ckpt",
        }

        self.flash_gradio.run(
            run_dict["task"],
            run_dict["checkpoint_path"],
        )

    def configure_layout(self):
        layout = [
            {
                "name": "Predictions Explorer (Gradio)",
                "content": self.flash_gradio,
            },
        ]
        return layout


# To launch the gradio component
app = LightningApp(FlashGradioComponent(), debug=True)
```
