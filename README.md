# CivicCare – Smart Public Complaint Management System

CivicCare is a desktop-based public complaint management application built with **Python and Tkinter**. Citizens can create accounts, submit public-issue complaints, and track their complaint history. An NLP-based machine-learning model predicts complaint categories and maps them to the relevant departments.

## Features

- Citizen registration and login with password hashing and verification.
- Complaint submission through a Tkinter graphical interface.
- NLP-based complaint classification using TF-IDF and Logistic Regression.
- Complaint validation before classification and submission.
- Duplicate detection for complaints submitted by the same user.
- Department mapping based on the predicted category.
- Complaint priority detection and prediction-confidence checking.
- Citizen complaint-history view.
- Administrator login and a separate administrator console.
- CSV-based storage for application records.

## Technologies

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| Tkinter / ttk | Desktop graphical user interface |
| pandas | Reading and handling dataset data |
| scikit-learn | TF-IDF and Logistic Regression workflow |
| CSV | Persistent storage of application records |

Tkinter is included with many Python installations. Some Linux distributions require a separate Tkinter system package.

## Project Structure

The main application imports supporting modules and reads a complaint dataset. Keep the required files together in the project directory unless paths are configured differently.

```text
CivicCare/
├── app.py                 # Main Tkinter application (entry point)
├── core.py                # Shared utilities, constants, file operations,
│                          # password utilities, priority detection and model training
├── validator.py           # Complaint validation
├── admin_panel.py         # Administrator interface
├── <complaint dataset>     # Training data for complaint classification
└── <CSV data files>        # User, admin, complaint and invalid-input records
```

**Important:** The supplied main script imports `core.py`, `validator.py`, and `admin_panel.py`. It also requires a complaint dataset. These files must be present and compatible with the main script. Confirm the exact dataset and CSV filenames from the project folder; they are configured in the supporting modules.

## Requirements

- Python 3.9 or later recommended
- `pandas`
- `scikit-learn`
- Tkinter

## Installation

1. Install Python and ensure it is available from your terminal.
2. Open a terminal or command prompt in the project directory.
3. (Recommended) Create and activate a virtual environment.

**Windows**
```bash
python -m venv .venv
.venv\\Scripts\\activate
```

**macOS / Linux**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

4. Install the required Python packages:

```bash
python -m pip install pandas scikit-learn
```

5. Confirm that all supporting Python modules and the complaint training dataset are in the expected locations.

## Running the Application

Run the following command from the project directory:

```bash
python app.py
```

The application initializes its data files, migrates the complaints file if required, trains the complaint-category model, prepares the validator, and opens the login screen.

If your main script has a different filename, replace `app.py` with that filename.

## How to Use

### Citizen workflow

1. Launch CivicCare.
2. Open **Create Account** and register with a username and password.
3. Return to **User Login** and sign in.
4. Open **Submit Complaint**.
5. Describe the public issue clearly, including what happened and where it happened.
6. Click **Analyse and Submit Complaint**.
7. Review the predicted category, department, priority, and confidence estimate.
8. Open **My Complaints** to view submitted complaints and refresh the list.
9. Select a complaint to view its available details and administrator remarks.

### Complaint processing

When a complaint is submitted, the application:

1. Validates the complaint text.
2. Checks whether the same user has already submitted an equivalent complaint.
3. Cleans/normalizes the text using the `clean_text` utility.
4. Predicts a complaint category using the trained model.
5. Obtains class probabilities and checks the maximum probability against the configured minimum confidence.
6. Maps the category to a department and determines priority.
7. Generates a complaint ID and saves the complaint with **Pending** status.
8. Refreshes the citizen's complaint history.

Complaints that fail validation, are duplicates, or do not meet the confidence threshold are not submitted.

### Administrator workflow

The login screen includes an **Admin Login** tab. Administrator functions are provided by `admin_panel.py`. Refer to that module for the administrator setup process and the exact actions available.

## NLP and Machine-Learning Methods

The main application uses these text-processing and classification steps:

- **Text cleaning/normalization:** performed by the `clean_text` utility imported from `core.py`.
- **TF-IDF vectorization:** represents complaint text as numerical features.
- **Logistic Regression:** predicts the complaint category.
- **Prediction probabilities:** the maximum predicted class probability is displayed as a confidence estimate.
- **Category-to-department mapping:** the predicted category is mapped using the configured department mapping.

The model is trained from the project's complaint dataset when the application starts. Confidence is a model estimate, not a guarantee that the prediction is correct.

## Data and Privacy

- The application uses CSV files for persistent records.
- Keep data files in the locations expected by `core.py`.
- Do not publish real users' personal information, password-related records, or complaint details in a public repository.
- Use test data for demonstrations.
- Back up CSV files before modifying application data.

## Troubleshooting

| Issue | What to check |
|---|---|
| `ModuleNotFoundError` for `core`, `validator`, or `admin_panel` | Ensure all supporting `.py` files are present in the project directory. |
| Dataset or model-training error | Ensure the complaint dataset exists and contains the `complaint` column read by the main script. Check the remaining expected columns in `core.py`. |
| `ModuleNotFoundError: pandas` | Run `python -m pip install pandas`. |
| `ModuleNotFoundError: sklearn` | Run `python -m pip install scikit-learn`. |
| Tkinter is unavailable | Install the Tkinter package for your operating system/Python distribution. |
| A complaint is rejected | Provide meaningful details and the issue location; review validation and confidence rules in the supporting modules. |

## Limitations

- Classification quality depends on the coverage, quality, and balance of the training dataset.
- The main GUI script is not sufficient by itself; supporting modules and the dataset are required.
- CSV storage is suitable for a small academic prototype, but is not a production-grade multi-user database.
- Validation and administrator behavior are implemented in separate modules and should be reviewed before deployment.

## Future Enhancements

- Add more complaint categories and expand the labelled training dataset.
- Evaluate the classifier using precision, recall, F1-score, and a confusion matrix.
- Replace CSV storage with a database.
- Add complaint-status notifications and reporting dashboards.
- Strengthen deployment security and administrator access management.

## License

No license is specified in the supplied source. Add a license file only after the project owner has selected the intended license.
