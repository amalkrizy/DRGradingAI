# DRGradingAI
Project repository utilizing computational methods for biomedical image analysis for the automatic grading of Diabetic Retinopathy in fundus eye images.
---------------------------------------------------------------------------------------------------------------------------------
This project was tested on Ubuntu 22.04 LTS with Python 3.10

### How to run the program:
1)Ensure CUDA is installed. If not, install here:
  - https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=10 
  - Choose your appropriate operating system and version.
  - Choose the installer type as (exe (network)).
 
2) Install python 3.10
3) Install everything you need
   - `python -m venv venv`
   - Activate the virtual environment
     - Linux/MacOS: `source venv/bin/activate`
     - Windows: `venv\Scripts\activate`
4) Install the model folder files, left-click extract and choose the venv\Scripts folder as the path to extract in:
   - E.g full path address C:\Users\"YourAccountName"\venv\Scripts
5) On terminal write: 'cd venv\Scripts' to enter onto the Scripts folder. 
   - 'pip install -r requirements.txt'
6) To setup the GPU version of pytorch, follow the instructions in this [link](https://github.com/openai/whisper/discussions/47).
     A quick summary of the steps is given below:
     - `pip uninstall torch`
     - `pip cache purge`
     - `pip3 install torch==1.13.1+cu117 torchvision>=0.13.1+cu117 torchaudio>=0.13.1+cu117 --extra-index-url https://download.pytorch.org/whl/cu117 --no-cache-dir`
     -  There should be a download size of 2.3GB if it is downloading the GPU version correctly. If it's something like 200MB, that's the CPU version.
     -  If that didn't work, simply install the CPU version for now. It'll slow down transcription but it'll work.
     -  `pip install torch`
           
7) `python source/run.py`
    - You can follow the on screen instructions to run the program.
    - If it shows up that some packages are missing, refer back to the requirements.txt file in the venv/Scripts folder to copy the package name. 
    - `pip install` again followed by package name. If it gives issues on some package names, try installing without version, 
      e.g instead of `pip install triton==2.0.0` use `pip install triton`
      
8)
### What the outputs and parameters from predict.py mean:

     - Predict class
        Class to help initialise the model and encapsulate the predict function. Use this class to use the predict function encapsulated within it.
        
        Parameters:
        ----------
        model_save_path: str
            path to the model to load
        device: str, optional
            device to run the model on. If None, will use cuda by default if available, else will use cpu.
            Options: "cpu" or "cuda".
        batch_size: int, optional
            batch size to use when predicting. Reduce if running out of memory when using cuda
        image_size: int, optional
            image size to use when predicting. Do not change unless you trained the model with a different image size
        verbose: bool, optional
            whether to print out information when predicting
        labels: list, optional
            list of labels to use when predicting. Do not change unless you trained the model with different labels
        best_hp_json_save_path: str, optional
            path to the json file containing the best hyperparameters from the tuning process
        num_classes: int, optional
            number of classes to use when predicting. Do not change unless you trained the model with a different number of classes

     - predict()
        function of Predict class in predict.py file is the main function that is called to predict the condition of the retina for the given image(s).
        Predict the condition of the retina for the given image(s)

        Parameters
        ----------
        images: str or list of str
            Argument can be one of the following:
                - string path to a single image
                - string path to a folder containing images
                - list of string paths to images

        Returns
        -------
        outputs: dict of str: int
            You'll get a dictionary of image paths and their corresponding predicted labels.
            
            Example:
                >>> Predict().predict(cf.INPUT_FOLDER)  # contains 3 images

                Output:
                    {'<cf.INPUT_FOLDER>/test_IDRiD_016-Severe-3.jpg': 'Severe',
                     '<cf.INPUT_FOLDER>/test_IDRiD_047-No_DR-0.jpg': 'No_DR',
                     '<cf.INPUT_FOLDER>/train_IDRiD_236-Moderate-2.jpg': 'Moderate'}


