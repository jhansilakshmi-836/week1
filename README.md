{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "2SWv_C1e5jdD"
      },
      "outputs": [],
      "source": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "\n",
        "Waste management using CNN model"
      ],
      "metadata": {
        "id": "IhDH-B0H7TLN"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "\n",
        "!pip install opencv-python\n",
        "!pip install tensorflow\n",
        "!pip install pandas\n",
        "!pip install numpy\n",
        "!pip install matplotlib\n",
        "!pip install tqdm\n",
        "!ls -l /content/\n",
        "!file /content/Dataset.rar"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "KNkHZUku7jV4",
        "outputId": "35129ccb-340d-47c0-85a4-ccd41cb1beb0"
      },
      "execution_count": 12,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Requirement already satisfied: opencv-python in /usr/local/lib/python3.11/dist-packages (4.10.0.84)\n",
            "Requirement already satisfied: numpy>=1.21.2 in /usr/local/lib/python3.11/dist-packages (from opencv-python) (1.26.4)\n",
            "Requirement already satisfied: tensorflow in /usr/local/lib/python3.11/dist-packages (2.17.1)\n",
            "Requirement already satisfied: absl-py>=1.0.0 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (1.4.0)\n",
            "Requirement already satisfied: astunparse>=1.6.0 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (1.6.3)\n",
            "Requirement already satisfied: flatbuffers>=24.3.25 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (24.12.23)\n",
            "Requirement already satisfied: gast!=0.5.0,!=0.5.1,!=0.5.2,>=0.2.1 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (0.6.0)\n",
            "Requirement already satisfied: google-pasta>=0.1.1 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (0.2.0)\n",
            "Requirement already satisfied: h5py>=3.10.0 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (3.12.1)\n",
            "Requirement already satisfied: libclang>=13.0.0 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (18.1.1)\n",
            "Requirement already satisfied: ml-dtypes<0.5.0,>=0.3.1 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (0.4.1)\n",
            "Requirement already satisfied: opt-einsum>=2.3.2 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (3.4.0)\n",
            "Requirement already satisfied: packaging in /usr/local/lib/python3.11/dist-packages (from tensorflow) (24.2)\n",
            "Requirement already satisfied: protobuf!=4.21.0,!=4.21.1,!=4.21.2,!=4.21.3,!=4.21.4,!=4.21.5,<5.0.0dev,>=3.20.3 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (4.25.5)\n",
            "Requirement already satisfied: requests<3,>=2.21.0 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (2.32.3)\n",
            "Requirement already satisfied: setuptools in /usr/local/lib/python3.11/dist-packages (from tensorflow) (75.1.0)\n",
            "Requirement already satisfied: six>=1.12.0 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (1.17.0)\n",
            "Requirement already satisfied: termcolor>=1.1.0 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (2.5.0)\n",
            "Requirement already satisfied: typing-extensions>=3.6.6 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (4.12.2)\n",
            "Requirement already satisfied: wrapt>=1.11.0 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (1.17.0)\n",
            "Requirement already satisfied: grpcio<2.0,>=1.24.3 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (1.69.0)\n",
            "Requirement already satisfied: tensorboard<2.18,>=2.17 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (2.17.1)\n",
            "Requirement already satisfied: keras>=3.2.0 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (3.5.0)\n",
            "Requirement already satisfied: tensorflow-io-gcs-filesystem>=0.23.1 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (0.37.1)\n",
            "Requirement already satisfied: numpy<2.0.0,>=1.23.5 in /usr/local/lib/python3.11/dist-packages (from tensorflow) (1.26.4)\n",
            "Requirement already satisfied: wheel<1.0,>=0.23.0 in /usr/local/lib/python3.11/dist-packages (from astunparse>=1.6.0->tensorflow) (0.45.1)\n",
            "Requirement already satisfied: rich in /usr/local/lib/python3.11/dist-packages (from keras>=3.2.0->tensorflow) (13.9.4)\n",
            "Requirement already satisfied: namex in /usr/local/lib/python3.11/dist-packages (from keras>=3.2.0->tensorflow) (0.0.8)\n",
            "Requirement already satisfied: optree in /usr/local/lib/python3.11/dist-packages (from keras>=3.2.0->tensorflow) (0.13.1)\n",
            "Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.11/dist-packages (from requests<3,>=2.21.0->tensorflow) (3.4.1)\n",
            "Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.11/dist-packages (from requests<3,>=2.21.0->tensorflow) (3.10)\n",
            "Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.11/dist-packages (from requests<3,>=2.21.0->tensorflow) (2.3.0)\n",
            "Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.11/dist-packages (from requests<3,>=2.21.0->tensorflow) (2024.12.14)\n",
            "Requirement already satisfied: markdown>=2.6.8 in /usr/local/lib/python3.11/dist-packages (from tensorboard<2.18,>=2.17->tensorflow) (3.7)\n",
            "Requirement already satisfied: tensorboard-data-server<0.8.0,>=0.7.0 in /usr/local/lib/python3.11/dist-packages (from tensorboard<2.18,>=2.17->tensorflow) (0.7.2)\n",
            "Requirement already satisfied: werkzeug>=1.0.1 in /usr/local/lib/python3.11/dist-packages (from tensorboard<2.18,>=2.17->tensorflow) (3.1.3)\n",
            "Requirement already satisfied: MarkupSafe>=2.1.1 in /usr/local/lib/python3.11/dist-packages (from werkzeug>=1.0.1->tensorboard<2.18,>=2.17->tensorflow) (3.0.2)\n",
            "Requirement already satisfied: markdown-it-py>=2.2.0 in /usr/local/lib/python3.11/dist-packages (from rich->keras>=3.2.0->tensorflow) (3.0.0)\n",
            "Requirement already satisfied: pygments<3.0.0,>=2.13.0 in /usr/local/lib/python3.11/dist-packages (from rich->keras>=3.2.0->tensorflow) (2.18.0)\n",
            "Requirement already satisfied: mdurl~=0.1 in /usr/local/lib/python3.11/dist-packages (from markdown-it-py>=2.2.0->rich->keras>=3.2.0->tensorflow) (0.1.2)\n",
            "Requirement already satisfied: pandas in /usr/local/lib/python3.11/dist-packages (2.2.2)\n",
            "Requirement already satisfied: numpy>=1.23.2 in /usr/local/lib/python3.11/dist-packages (from pandas) (1.26.4)\n",
            "Requirement already satisfied: python-dateutil>=2.8.2 in /usr/local/lib/python3.11/dist-packages (from pandas) (2.8.2)\n",
            "Requirement already satisfied: pytz>=2020.1 in /usr/local/lib/python3.11/dist-packages (from pandas) (2024.2)\n",
            "Requirement already satisfied: tzdata>=2022.7 in /usr/local/lib/python3.11/dist-packages (from pandas) (2024.2)\n",
            "Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.11/dist-packages (from python-dateutil>=2.8.2->pandas) (1.17.0)\n",
            "Requirement already satisfied: numpy in /usr/local/lib/python3.11/dist-packages (1.26.4)\n",
            "Requirement already satisfied: matplotlib in /usr/local/lib/python3.11/dist-packages (3.10.0)\n",
            "Requirement already satisfied: contourpy>=1.0.1 in /usr/local/lib/python3.11/dist-packages (from matplotlib) (1.3.1)\n",
            "Requirement already satisfied: cycler>=0.10 in /usr/local/lib/python3.11/dist-packages (from matplotlib) (0.12.1)\n",
            "Requirement already satisfied: fonttools>=4.22.0 in /usr/local/lib/python3.11/dist-packages (from matplotlib) (4.55.3)\n",
            "Requirement already satisfied: kiwisolver>=1.3.1 in /usr/local/lib/python3.11/dist-packages (from matplotlib) (1.4.8)\n",
            "Requirement already satisfied: numpy>=1.23 in /usr/local/lib/python3.11/dist-packages (from matplotlib) (1.26.4)\n",
            "Requirement already satisfied: packaging>=20.0 in /usr/local/lib/python3.11/dist-packages (from matplotlib) (24.2)\n",
            "Requirement already satisfied: pillow>=8 in /usr/local/lib/python3.11/dist-packages (from matplotlib) (11.1.0)\n",
            "Requirement already satisfied: pyparsing>=2.3.1 in /usr/local/lib/python3.11/dist-packages (from matplotlib) (3.2.1)\n",
            "Requirement already satisfied: python-dateutil>=2.7 in /usr/local/lib/python3.11/dist-packages (from matplotlib) (2.8.2)\n",
            "Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.11/dist-packages (from python-dateutil>=2.7->matplotlib) (1.17.0)\n",
            "Requirement already satisfied: tqdm in /usr/local/lib/python3.11/dist-packages (4.67.1)\n",
            "total 4\n",
            "drwxr-xr-x 1 root root 4096 Jan 22 14:23 sample_data\n",
            "/content/Dataset.rar: cannot open `/content/Dataset.rar' (No such file or directory)\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "import pandas as pd\n",
        "import numpy as np\n",
        "import matplotlib.pyplot as plt\n",
        "import cv2\n",
        "from tqdm import tqdm\n",
        "import warnings\n",
        "warnings.filterwarnings('ignore')"
      ],
      "metadata": {
        "id": "LT2QPezt8R0P"
      },
      "execution_count": 11,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "train_path = \"/content/DATASET.rar\"\n",
        "test_path = \"/content/DATASET.rar\"\n",
        ""
      ],
      "metadata": {
        "id": "WEd2-nHp8ZK2"
      },
      "execution_count": 10,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "from tensorflow.keras.models import Sequential\n",
        "from tensorflow.keras.layers import Conv2D, MaxPooling2D, Activation, Dropout, Flatten, Dense, BatchNormalization\n",
        "from tensorflow.keras.preprocessing.image import ImageDataGenerator, img_to_array, load_img\n",
        "from tensorflow.keras.utils import plot_model # Corrected the import statement from 'utlis' to 'utils'\n",
        "from glob import glob\n",
        ""
      ],
      "metadata": {
        "id": "PD0hwNSF8kub"
      },
      "execution_count": 9,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "from cv2 import cvtColor\n",
        "x_data = []\n",
        "y_data = []\n",
        "for category in glob(train_path+'/*'):\n",
        "  for file in tqdm(glob(category+'/*')):\n",
        "    img_array = cv2.imread(file)\n",
        "    img_array = cv2.cvtColor(img_array, cv2.COLOR_BGR2RGB)\n",
        "    x_data.append(img_array)\n",
        "    y_data.append(category.split('/')[-1])\n",
        "data = pd.DataFrame({'image':x_data, 'label':y_data})\n",
        "print(data.shape)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "u8pwe9ix8vCW",
        "outputId": "f0b1ea0d-5e44-498c-a5ef-0999be84c026"
      },
      "execution_count": 8,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "(0, 2)\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "import matplotlib.pyplot as plt\n",
        "import numpy as np\n",
        "\n",
        "# Total count (chosen for precision)\n",
        "total_items = 10000  # Large number to ensure precision\n",
        "organic_count = round(55.69 / 100 * total_items)  # Use round to avoid precision errors\n",
        "recyclable_count = total_items - organic_count  # Remaining count ensures exact proportions\n",
        "\n",
        "# Print percentages for verification\n",
        "print(f\"Organic percentage: {(organic_count / total_items) * 100:.2f}%\")\n",
        "print(f\"Recyclable percentage: {(recyclable_count / total_items) * 100:.2f}%\")\n",
        "\n",
        "# Simulated data\n",
        "counts = [organic_count, recyclable_count]\n",
        "labels = ['organic', 'recyclable']\n",
        "\n",
        "# Colors for the pie chart\n",
        "colors = plt.cm.Paired(np.linspace(0, 1, len(counts)))\n",
        "\n",
        "# Exploding the slices slightly\n",
        "explode = [0.05] * len(counts)\n",
        "\n",
        "# Plotting the pie chart\n",
        "plt.pie(counts, labels=labels, autopct='%0.2f%%', colors=colors, startangle=90, explode=explode)\n",
        "plt.title('Distribution of Organic and Recyclable Items')  # Optional title\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 463
        },
        "id": "7QncvveQ83JX",
        "outputId": "16aaec11-c370-428d-c444-4342edb1858c"
      },
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Organic percentage: 55.69%\n",
            "Recyclable percentage: 44.31%\n"
          ]
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ],
            "image/png": "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KnTq1AlbtmzB8ePHMXnyZDz22GP480/TM/3PPvsMLi4u+PPPP7F06VIsWrQIq1atqvNnvv/++5GTk4OtW7fi4MGD6NixIwYMGICCgvovnygROBN2gx25WoxT2aVil0FNlL+7HHe2DDLrsalLUnDtz+1WroicSe93v4JvbFuLjpGcnIySkhIcOnQIADBz5kz88ssv2L79n/dmZmYmIiMjcebMGYSGhiIwMBDvv/8+Jk6cWON4VVVVCAsLw8qVK41B3b59e9xzzz14++23cfbsWbRs2RI7d+7EwIEDazx+79696NevHwoLC+Hj41NrzcOHD0dCQgIWLFhg/BlycnJw4sQJSCQSAMBrr72GzZs34+TJ6y0CMTExSElJQUpKCn799VcMGzYMOTk5UCgUxuPGxcXhlVdeweTJk+v12rnUay8yUcAzXSJqooozTlocugDQqVMn47+PHj2KPXv21HrmmZaWhqKiIqjVagwYMKDWYymVSjz22GP45JNP8MADD+DQoUM4fvw4Nm/eDAA4cuQIZDIZ+vbtW6/a9Ho9Zs+eja+++gpXrlyBRqOBWq2Gu7u7yX533HGHMXABoHv37li4cCH0ej1kMpnJvkePHkVZWRn8/f1NtldWViItLa1edQEM3QYTBAEFlQxdImqaiq10XVelUhn/XVZWhhEjRmDevHk19gsNDcWFCxdue7yJEyciKSkJmZmZ+PTTT9G/f39ER0cDANzc3BpU2/z587F06VIsWbIEbdu2hUqlQkpKikUdvsrKyhAaGoq9e/fWuK+us+vaMHQbqFStg1bPFnkiapqKM6wTujfq2LEjvv76a8TExMDFpWasxMfHw83NDbt27aq1eRkA2rZti86dO+Ojjz7C+vXr8f7775vcZzAY8NNPP9XavHyzffv2YdSoUXj00UcBXO/EdfbsWSQmJprs98cff5jc/v333xEfH1/jLLf6Z8zKyoKLiwtiYmJuW0Nd2JGqgQoqOAsVETVdpVfSrL6o/TPPPIOCggKMGTMGqampSEtLw/bt2zFhwgTo9XoolUq8+uqreOWVV/D5558jLS0Nv//+e43exxMnTsTcuXMhCALuvvtu4/aYmBiMGzcOjz/+OL799lukp6dj7969+Oqrr2qtJz4+Hjt37sRvv/2GU6dO4cknn0R2dnaN/S5duoQXXngBZ86cwRdffIHly5fjueee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\n"
          },
          "metadata": {}
        }
      ]
    }
  ]
}
