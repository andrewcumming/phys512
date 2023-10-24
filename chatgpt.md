# ChatGPT 'Real-Life Example'

#### You:
I have a list of links to files I need to download in a text file where each link is seperated by a new line. I want to download all of these files in python and then create a numpy array containing a list of these files. 

#### ChatGPT:
To download files from a list of links and create a NumPy array in Python, you can use the `requests` library to download the files and then use NumPy to work with the data. First, you need to install the `requests` library if you haven't already. You can install it using `pip`:

```bash
pip install requests
```

Then, you can use the following Python code to download the files and create a NumPy array from them:

```python
import requests
import numpy as np
from io import BytesIO

# Read the list of links from a text file (one link per line)
file_path = 'list_of_links.txt'
with open(file_path, 'r') as file:
    links = file.read().splitlines()

# Initialize an empty list to store the downloaded file data
file_data_list = []

# Download the files and store their content in the list
for link in links:
    response = requests.get(link)
    if response.status_code == 200:
        file_data_list.append(response.content)
    else:
        print(f"Failed to download: {link}")

# Convert the list of file data into a NumPy array
file_data_array = np.array(file_data_list, dtype=object)

# Now you have a NumPy array containing the data of the downloaded files
```

This code reads the list of links from the text file 'list\_of\_links.txt', downloads the files, and stores their content in a list. Then, it converts this list into a NumPy array, where each element of the array contains the data of one downloaded file. Make sure to replace 'list\_of\_links.txt' with the actual path to your text file containing the links.


#### You:
can you download them into a subfolder called data

#### ChatGPT:
Sure, you can download the files into a subfolder called 'data' by modifying the code as follows:

```python
import os
import requests
import numpy as np
from io import BytesIO

# Specify the subfolder name
subfolder = 'data'

# Create the subfolder if it doesn't exist
if not os.path.exists(subfolder):
    os.mkdir(subfolder)

# Read the list of links from a text file (one link per line)
file_path = 'list_of_links.txt'
with open(file_path, 'r') as file:
    links = file.read().splitlines()

# Initialize an empty list to store the downloaded file data
file_data_list = []

# Download the files into the 'data' subfolder and store their content in the list
for link in links:
    response = requests.get(link)
    if response.status_code == 200:
        file_name = os.path.join(subfolder, os.path.basename(link))
        with open(file_name, 'wb') as file:
            file.write(response.content)
        file_data_list.append(response.content)
    else:
        print(f"Failed to download: {link}")

# Convert the list of file data into a NumPy array
file_data_array = np.array(file_data_list, dtype=object)

# Now you have a NumPy array containing the data of the downloaded files in the 'data' subfolder
```

In this code, we create the 'data' subfolder if it doesn't already exist using `os.mkdir`. Then, the downloaded files are saved into this subfolder with their original filenames using `os.path.basename(link)` to extract the filename from the URL. The rest of the code remains the same, and you will have a NumPy array containing the data of the downloaded files in the 'data' subfolder.


#### You:
ok can you do this in a function and clean up the style 

#### ChatGPT:
Certainly, here's a cleaner and more organized version of the code encapsulated within a function:

```python
import os
import requests
import numpy as np

def download_files_from_links(link_file_path, subfolder='data'):
    # Create the subfolder if it doesn't exist
    if not os.path.exists(subfolder):
        os.mkdir(subfolder)

    # Read the list of links from a text file (one link per line)
    with open(link_file_path, 'r') as file:
        links = file.read().splitlines()

    # Initialize an empty list to store the downloaded file data
    file_data_list = []

    for link in links:
        response = requests.get(link)
        if response.status_code == 200:
            file_name = os.path.join(subfolder, os.path.basename(link))
            with open(file_name, 'wb') as file:
                file.write(response.content)
            file_data_list.append(response.content)
        else:
            print(f"Failed to download: {link}")

    # Convert the list of file data into a NumPy array
    file_data_array = np.array(file_data_list, dtype=object)

    return file_data_array

if __name__ == "__main__":
    link_file_path = 'list_of_links.txt'
    subfolder = 'data'
    downloaded_files = download_files_from_links(link_file_path, subfolder)
    print(f"Downloaded {len(downloaded_files)} files.")
```

This code defines a function `download_files_from_links` that takes the path to the file containing the links and an optional subfolder name as arguments. The function encapsulates the logic for downloading files and organizing them into the specified subfolder. The cleaned-up code makes it easier to reuse and maintain.


#### You:
ok all of these files are fits files with tabular data, after they are download  can you import them with astropy and then plot the light curve data

#### ChatGPT:
Certainly, you can use the Astropy library to import FITS files containing tabular data and then plot the light curve data. Here's how you can modify the code to achieve this:

```python
import os
import requests
import numpy as np
import matplotlib.pyplot as plt
from astropy.io import fits

def download_and_plot_light_curves(link_file_path, subfolder='data'):
    # Create the subfolder if it doesn't exist
    if not os.path.exists(subfolder):
        os.mkdir(subfolder)

    # Read the list of links from a text file (one link per line)
    with open(link_file_path, 'r') as file:
        links = file.read().splitlines()

    # Initialize an empty list to store the light curve data
    light_curves = []

    for link in links:
        response = requests.get(link)
        if response.status_code == 200:
            file_name = os.path.join(subfolder, os.path.basename(link))
            with open(file_name, 'wb') as file:
                file.write(response.content)
            light_curve = fits.open(file_name)  # Open the FITS file
            light_curves.append(light_curve)
        else:
            print(f"Failed to download: {link}")

    # Plot the light curve data
    for i, light_curve in enumerate(light_curves):
        data = light_curve[1].data  # Assuming the table is in the first HDU
        time = data['TIME']  # Replace 'TIME' with the actual column name
        flux = data['FLUX']  # Replace 'FLUX' with the actual column name

        plt.figure(i + 1)
        plt.scatter(time, flux, s=1, marker='.', label=f'File {i + 1}')
        plt.xlabel('Time')
        plt.ylabel('Flux')
        plt.title(f'Light Curve {i + 1}')
        plt.legend()

    plt.show()

if __name__ == "__main__":
    link_file_path = 'list_of_links.txt'
    subfolder = 'data'
    download_and_plot_light_curves(link_file_path, subfolder)
```

In this modified code, we open each downloaded FITS file using `fits.open()` from Astropy and extract the relevant data from the table (replace 'TIME' and 'FLUX' with the actual column names in your FITS files). We then use Matplotlib to create scatter plots of the light curves, one for each downloaded file. The `plt.show()` function displays the plots.

Make sure to adapt the code by replacing 'TIME' and 'FLUX' with the actual column names from your FITS files. This code will create individual plots for each FITS file in the subfolder 'data'.


#### You:

can you split the download and plot into two seperate functions where the light curve list is returned to the main function

#### ChatGPT:
Sure, we can split the download and plotting into two separate functions and have the light curve list returned to the main function. Here's the modified code:

```python
import os
import requests
import numpy as np
import matplotlib.pyplot as plt
from astropy.io import fits

def download_files(link_file_path, subfolder='data'):
    # Create the subfolder if it doesn't exist
    if not os.path.exists(subfolder):
        os.mkdir(subfolder)

    # Read the list of links from a text file (one link per line)
    with open(link_file_path, 'r') as file:
        links = file.read().splitlines()

    # Initialize an empty list to store the downloaded file names
    downloaded_files = []

    for link in links:
        response = requests.get(link)
        if response.status_code == 200:
            file_name = os.path.join(subfolder, os.path.basename(link))
            with open(file_name, 'wb') as file:
                file.write(response.content)
            downloaded_files.append(file_name)
        else:
            print(f"Failed to download: {link}")

    return downloaded_files

def plot_light_curves(downloaded_files):
    light_curves = []
    for file_path in downloaded_files:
        light_curve = fits.open(file_path)
        light_curves.append(light_curve)

    # Plot the light curve data
    for i, light_curve in enumerate(light_curves):
        data = light_curve[1].data  # Assuming the table is in the first HDU
        time = data['TIME']  # Replace 'TIME' with the actual column name
        flux = data['FLUX']  # Replace 'FLUX' with the actual column name

        plt.figure(i + 1)
        plt.scatter(time, flux, s=1, marker='.', label=f'File {i + 1}')
        plt.xlabel('Time')
        plt.ylabel('Flux')
        plt.title(f'Light Curve {i + 1}')
        plt.legend()

    plt.show()

if __name__ == "__main__":
    link_file_path = 'list_of_links.txt'
    subfolder = 'data'

    # Download the files and get a list of downloaded file paths
    downloaded_files = download_files(link_file_path, subfolder)

    # Plot the light curves using the downloaded file paths
    plot_light_curves(downloaded_files)
```

In this code, the `download_files` function downloads the files and returns a list of the downloaded file paths, and the `plot_light_curves` function takes this list of file paths to plot the light curves. This separation of functionality allows for better code organization and reusability.


#### You:
i have an apache host that I'm trying to post the plots to a website, can you write another function that runs after the plots have all been produced and saved as PNGs and then hosts these plots online

#### ChatGPT:
Certainly, you can use the `http.server` module in Python to serve the PNG plots through a local HTTP server. To do this, you can create a new function that runs after the plots have been produced and saved as PNGs. Here's a simple example of how you can achieve this:

```python
import os
import requests
import numpy as np
import matplotlib.pyplot as plt
from astropy.io import fits
import http.server
import socketserver

def download_files(link_file_path, subfolder='data'):
    # Create the subfolder if it doesn't exist
    if not os.path.exists(subfolder):
        os.mkdir(subfolder)

    # Read the list of links from a text file (one link per line)
    with open(link_file_path, 'r') as file:
        links = file.read().splitlines()

    # Initialize an empty list to store the downloaded file names
    downloaded_files = []

    for link in links:
        response = requests.get(link)
        if response.status_code == 200:
            file_name = os.path.join(subfolder, os.path.basename(link))
            with open(file_name, 'wb') as file:
                file.write(response.content)
            downloaded_files.append(file_name)
        else:
            print(f"Failed to download: {link}")

    return downloaded_files

def plot_light_curves(downloaded_files, output_folder='plots'):
    # Create the output folder for saving plots
    if not os.path.exists(output_folder):
        os.mkdir(output_folder)

    plot_file_paths = []

    for i, file_path in enumerate(downloaded_files):
        light_curve = fits.open(file_path)
        data = light_curve[1].data  # Assuming the table is in the first HDU
        time = data['TIME']  # Replace 'TIME' with the actual column name
        flux = data['FLUX']  # Replace 'FLUX' with the actual column name

        plt.figure(i + 1)
        plt.scatter(time, flux, s=1, marker='.', label=f'File {i + 1}')
        plt.xlabel('Time')
        plt.ylabel('Flux')
        plt.title(f'Light Curve {i + 1}')
        plt.legend()

        # Save the plot as a PNG
        plot_file_path = os.path.join(output_folder, f'plot_{i + 1}.png')
        plt.savefig(plot_file_path)
        plot_file_paths.append(plot_file_path)

        plt.close()

    return plot_file_paths

def serve_plots(plot_file_paths, port=8080):
    Handler = http.server.SimpleHTTPRequestHandler
    with socketserver.TCPServer(("", port), Handler) as httpd:
        print(f"Serving plots at http://localhost:{port}")
        for plot_file_path in plot_file_paths:
            os.replace(plot_file_path, os.path.join(os.getcwd(), plot_file_path))
        httpd.serve_forever()

if __name__ == "__main__":
    link_file_path = 'list_of_links.txt'
    subfolder = 'data'

    # Download the files and get a list of downloaded file paths
    downloaded_files = download_files(link_file_path, subfolder)

    # Plot the light curves and get a list of plot file paths
    plot_file_paths = plot_light_curves(downloaded_files, output_folder='plots')

    # Serve the plots through a local HTTP server
    serve_plots(plot_file_paths)
```

In this code, the `serve_plots` function sets up a simple HTTP server using `http.server` to serve the saved PNG plots. You can access the plots by opening a web browser and navigating to `http://localhost:8080` (or replace `8080` with the desired port number).

Make sure to adapt the code to your specific needs, including configuring the port number and setting the appropriate output folder for saving the plots.


#### You:
can you instead save them as embeded in  an html file

#### ChatGPT:
Certainly, you can save the plots as embedded images in an HTML file. Here's how you can modify the code to achieve this:

```python
import os
import requests
import numpy as np
import matplotlib.pyplot as plt
from astropy.io import fits
from io import BytesIO
import base64

def download_files(link_file_path, subfolder='data'):
    # Create the subfolder if it doesn't exist
    if not os.path.exists(subfolder):
        os.mkdir(subfolder)

    # Read the list of links from a text file (one link per line)
    with open(link_file_path, 'r') as file:
        links = file.read().splitlines()

    # Initialize an empty list to store the downloaded file names
    downloaded_files = []

    for link in links:
        response = requests.get(link)
        if response.status_code == 200:
            file_name = os.path.join(subfolder, os.path.basename(link))
            with open(file_name, 'wb') as file:
                file.write(response.content)
            downloaded_files.append(file_name)
        else:
            print(f"Failed to download: {link}")

    return downloaded_files

def plot_light_curves(downloaded_files):
    plot_images = []

    for i, file_path in enumerate(downloaded_files):
        light_curve = fits.open(file_path)
        data = light_curve[1].data  # Assuming the table is in the first HDU
        time = data['TIME']  # Replace 'TIME' with the actual column name
        flux = data['FLUX']  # Replace 'FLUX' with the actual column name

        plt.figure(i + 1)
        plt.scatter(time, flux, s=1, marker='.', label=f'File {i + 1}')
        plt.xlabel('Time')
        plt.ylabel('Flux')
        plt.title(f'Light Curve {i + 1}')
        plt.legend()

        # Convert the plot to a base64-encoded image
        buffer = BytesIO()
        plt.savefig(buffer, format='png')
        buffer.seek(0)
        image_data = base64.b64encode(buffer.read()).decode()
        plot_images.append(image_data)

        plt.close()

    return plot_images

def create_html_with_plots(plot_images, output_file='plots.html'):
    with open(output_file, 'w') as html_file:
        html_file.write('<html><body>\n')

        for i, image_data in enumerate(plot_images):
            html_file.write(f'<h2>Plot {i + 1}</h2>\n')
            html_file.write(f'<img src="data:image/png;base64, {image_data}" />\n')

        html_file.write('</body></html>')

if __name__ == "__main__":
    link_file_path = 'list_of_links.txt'
    subfolder = 'data'

    # Download the files and get a list of downloaded file paths
    downloaded_files = download_files(link_file_path, subfolder)

    # Plot the light curves and get a list of base64-encoded plot images
    plot_images = plot_light_curves(downloaded_files)

    # Create an HTML file with embedded plots
    create_html_with_plots(plot_images)
```

In this modified code, the `plot_light_curves` function converts each plot to a base64-encoded image and stores them in a list. The `create_html_with_plots` function creates an HTML file with embedded images using the base64-encoded data. You can specify the output file name in the `output_file` argument. When you run this code, it will create an HTML file with embedded plots.


#### You:
great can you create a header on the HTML file with the following information (you can reword it: LMXB Trigger for VERITAS, this website shows the Swift-BAT daily lightcurves for the LMXBs visible to veritas. If a source is triggered then we will observe the source with the ECM. 

then organize the plots so that they are aesthetically well organized. 

#### ChatGPT:
Certainly, you can add a header to the HTML file and organize the embedded plots for a more aesthetically pleasing presentation. Here's the updated code:

```python
import os
import requests
import numpy as np
import matplotlib.pyplot as plt
from astropy.io import fits
from io import BytesIO
import base64

def download_files(link_file_path, subfolder='data'):
    # Create the subfolder if it doesn't exist
    if not os.path.exists(subfolder):
        os.mkdir(subfolder)

    # Read the list of links from a text file (one link per line)
    with open(link_file_path, 'r') as file:
        links = file.read().splitlines()

    # Initialize an empty list to store the downloaded file names
    downloaded_files = []

    for link in links:
        response = requests.get(link)
        if response.status_code == 200:
            file_name = os.path.join(subfolder, os.path.basename(link))
            with open(file_name, 'wb') as file:
                file.write(response.content)
            downloaded_files.append(file_name)
        else:
            print(f"Failed to download: {link}")

    return downloaded_files

def plot_light_curves(downloaded_files):
    plot_images = []

    for i, file_path in enumerate(downloaded_files):
        light_curve = fits.open(file_path)
        data = light_curve[1].data  # Assuming the table is in the first HDU
        time = data['TIME']  # Replace 'TIME' with the actual column name
        flux = data['FLUX']  # Replace 'FLUX' with the actual column name

        plt.figure(i + 1)
        plt.scatter(time, flux, s=1, marker='.', label=f'File {i + 1}')
        plt.xlabel('Time')
        plt.ylabel('Flux')
        plt.title(f'Light Curve {i + 1}')
        plt.legend()

        # Convert the plot to a base64-encoded image
        buffer = BytesIO()
        plt.savefig(buffer, format='png')
        buffer.seek(0)
        image_data = base64.b64encode(buffer.read()).decode()
        plot_images.append(image_data)

        plt.close()

    return plot_images

def create_html_with_plots(plot_images, output_file='plots.html'):
    with open(output_file, 'w') as html_file:
        html_file.write('<html><head><title>LMXB Trigger for VERITAS</title></head><body>\n')
        html_file.write('<h1>LMXB Trigger for VERITAS</h1>\n')
        html_file.write('<p>This website shows the Swift-BAT daily lightcurves for the LMXBs visible to VERITAS. If a source is triggered, then we will observe the source with the ECM.</p>\n')

        for i, image_data in enumerate(plot_images):
            if i % 2 == 0:
                html_file.write('<div style="display: flex; flex-direction: row;">\n')
            html_file.write('<div style="margin: 10px;">\n')
            html_file.write(f'<h2>Plot {i + 1}</h2>\n')
            html_file.write(f'<img src="data:image/png;base64, {image_data}" />\n')
            html_file.write('</div>\n')
            if i % 2 == 1 or i == len(plot_images) - 1:
                html_file.write('</div>\n')

        html_file.write('</body></html>')

if __name__ == "__main__":
    link_file_path = 'list_of_links.txt'
    subfolder = 'data'

    # Download the files and get a list of downloaded file paths
    downloaded_files = download_files(link_file_path, subfolder)

    # Plot the light curves and get a list of base64-encoded plot images
    plot_images = plot_light_curves(downloaded_files)

    # Create an HTML file with embedded plots and a header
    create_html_with_plots(plot_images)
```

In this updated code, the HTML file is generated with a title, header, and a more aesthetically organized layout for the embedded plots. The plots are organized in rows with a maximum of two plots per row. You can further customize the HTML layout as needed to achieve the desired appearance.


#### You:
Ok a couple of details, make the default link file path "download.txt" Change the x-axis label to "Time (MJD)", label the files in the HTML file and in the file themselves with the file name (excluding the .fits at the end). Change the ylabel to be r"Swift-BAT (15-50 keV) [Counts/cm$^2$/s]". Also change the scatter to an errorbar plot with no connecting line and a small alpha (0.8). If you could also make each plot a different color. 

For the fits column import if you could change "FLUX" to "RATE" and also import "ERROR" that would be great

#### ChatGPT:
Certainly, here's the modified code with your requested changes:

```python
import os
import requests
import numpy as np
import matplotlib.pyplot as plt
from astropy.io import fits
from io import BytesIO
import base64

def download_files(link_file_path='download.txt', subfolder='data'):
    # Create the subfolder if it doesn't exist
    if not os.path.exists(subfolder):
        os.mkdir(subfolder)

    # Read the list of links from a text file (one link per line)
    with open(link_file_path, 'r') as file:
        links = file.read().splitlines()

    # Initialize an empty list to store the downloaded file names
    downloaded_files = []

    for link in links:
        response = requests.get(link)
        if response.status_code == 200:
            file_name = os.path.join(subfolder, os.path.basename(link))
            with open(file_name, 'wb') as file:
                file.write(response.content)
            downloaded_files.append(file_name)
        else:
            print(f"Failed to download: {link}")

    return downloaded_files

def plot_light_curves(downloaded_files):
    plot_images = []

    for i, file_path in enumerate(downloaded_files):
        light_curve = fits.open(file_path)
        data = light_curve[1].data  # Assuming the table is in the first HDU
        time = data['TIME']  # Change 'TIME' to 'Time (MJD)'
        rate = data['RATE']  # Change 'FLUX' to 'RATE'
        error = data['ERROR']  # New column 'ERROR'

        plt.figure(i + 1)
        plt.errorbar(time, rate, yerr=error, fmt='o', markersize=2, alpha=0.8, label=f'{os.path.splitext(os.path.basename(file_path))[0]}', color=f'C{i%10}')
        plt.xlabel('Time (MJD)')
        plt.ylabel(r'Swift-BAT (15-50 keV) [Counts/cm$^2$/s]')
        plt.title(f'Light Curve {os.path.splitext(os.path.basename(file_path))[0]}')
        plt.legend()

        # Convert the plot to a base64-encoded image
        buffer = BytesIO()
        plt.savefig(buffer, format='png')
        buffer.seek(0)
        image_data = base64.b64encode(buffer.read()).decode()
        plot_images.append(image_data)

        plt.close()

    return plot_images

def create_html_with_plots(plot_images, output_file='plots.html'):
    with open(output_file, 'w') as html_file:
        html_file.write('<html><head><title>LMXB Trigger for VERITAS</title></head><body>\n')
        html_file.write('<h1>LMXB Trigger for VERITAS</h1>\n')
        html_file.write('<p>This website shows the Swift-BAT daily light curves for the LMXBs visible to VERITAS. If a source is triggered, then we will observe the source with the ECM.</p>\n')

        for i, image_data in enumerate(plot_images):
            if i % 2 == 0:
                html_file.write('<div style="display: flex; flex-direction: row;">\n')
            html_file.write('<div style="margin: 10px;">\n')
            html_file.write(f'<h2>{os.path.splitext(os.path.basename(downloaded_files[i]))[0]}</h2>\n')
            html_file.write(f'<img src="data:image/png;base64, {image_data}" />\n')
            html_file.write('</div>\n')
            if i % 2 == 1 or i == len(plot_images) - 1:
                html_file.write('</div>\n')

        html_file.write('</body></html>')

if __name__ == "__main__":
    subfolder = 'data'

    # Download the files and get a list of downloaded file paths
    downloaded_files = download_files(subfolder=subfolder)

    # Plot the light curves and get a list of base64-encoded plot images
    plot_images = plot_light_curves(downloaded_files)

    # Create an HTML file with embedded plots, header, and labels
    create_html_with_plots(plot_images)
```

In this updated code:

- The default link file path is set to "download.txt."
- The x-axis label is changed to "Time (MJD)."
- The files are labeled in the HTML file and in the filenames themselves.
- The y-axis label is set to "Swift-BAT (15-50 keV) \[Counts/cm²/s]."
- The plots are created as errorbar plots with no connecting lines and with different colors.
- The 'FLUX' column is imported as 'RATE,' and the 'ERROR' column is also imported from the FITS files.


#### You:
Great! Can you add a horizontal line in every plot at 0.2 and then if any of the last 7 points are above the line can you change the HTML title (the h2) of the image to be red. Can you also return to the main function a list of all files where the last flux point is above 0.2? 

#### ChatGPT:
To add a horizontal line at 0.2 to each plot and change the HTML title to red if any of the last 7 points are above the line, you can modify the code as follows:

```python
import os
import requests
import numpy as np
import matplotlib.pyplot as plt
from astropy.io import fits
from io import BytesIO
import base64

def download_files(link_file_path='download.txt', subfolder='data'):
    # Create the subfolder if it doesn't exist
    if not os.path.exists(subfolder):
        os.mkdir(subfolder)

    # Read the list of links from a text file (one link per line)
    with open(link_file_path, 'r') as file:
        links = file.read().splitlines()

    # Initialize an empty list to store the downloaded file names
    downloaded_files = []

    for link in links:
        response = requests.get(link)
        if response.status_code == 200:
            file_name = os.path.join(subfolder, os.path.basename(link))
            with open(file_name, 'wb') as file:
                file.write(response.content)
            downloaded_files.append(file_name)
        else:
            print(f"Failed to download: {link}")

    return downloaded_files

def check_last_flux_above_threshold(rate, threshold=0.2, last_n_points=7):
    return any(rate[-last_n_points:] > threshold)

def plot_light_curves(downloaded_files):
    plot_images = []
    files_above_threshold = []

    for i, file_path in enumerate(downloaded_files):
        light_curve = fits.open(file_path)
        data = light_curve[1].data
        time = data['TIME']
        rate = data['RATE']
        error = data['ERROR']

        # Determine if the last 7 points are above the threshold
        last_points_above_threshold = check_last_flux_above_threshold(rate)

        plt.figure(i + 1)
        plt.errorbar(time, rate, yerr=error, fmt='o', markersize=2, alpha=0.8, label=f'{os.path.splitext(os.path.basename(file_path))[0]}', color=f'C{i%10}')
        plt.axhline(y=0.2, color='red', linestyle='--')
        plt.xlabel('Time (MJD)')
        plt.ylabel(r'Swift-BAT (15-50 keV) [Counts/cm$^2$/s]')
        plt.title(f'Light Curve {os.path.splitext(os.path.basename(file_path))[0]}' + (" (Above Threshold)" if last_points_above_threshold else ""))
        plt.legend()

        # Convert the plot to a base64-encoded image
        buffer = BytesIO()
        plt.savefig(buffer, format='png')
        buffer.seek(0)
        image_data = base64.b64encode(buffer.read()).decode()
        plot_images.append(image_data)

        plt.close()

        if last_points_above_threshold:
            files_above_threshold.append(file_path)

    return plot_images, files_above_threshold

def create_html_with_plots(plot_images, output_file='plots.html'):
    with open(output_file, 'w') as html_file:
        html_file.write('<html><head><title>LMXB Trigger for VERITAS</title></head><body>\n')
        html_file.write('<h1>LMXB Trigger for VERITAS</h1>\n')
        html_file.write('<p>This website shows the Swift-BAT daily light curves for the LMXBs visible to VERITAS. If a source is triggered, then we will observe the source with the ECM.</p>\n')

        for i, image_data in enumerate(plot_images):
            if i % 2 == 0:
                html_file.write('<div style="display: flex; flex-direction: row;">\n')
            html_file.write('<div style="margin: 10px;">\n')
            html_file.write(f'<h2 style="color: red;">{os.path.splitext(os.path.basename(downloaded_files[i]))[0]}</h2>\n')
            html_file.write(f'<img src="data:image/png;base64, {image_data}" />\n')
            html_file.write('</div>\n')
            if i % 2 == 1 or i == len(plot_images) - 1:
                html_file.write('</div>\n')

        html_file.write('</body></html>')

if __name__ == "__main__":
    subfolder = 'data'

    # Download the files and get a list of downloaded file paths
    downloaded_files = download_files(subfolder=subfolder)

    # Plot the light curves and get a list of base64-encoded plot images and files above threshold
    plot_images, files_above_threshold = plot_light_curves(downloaded_files)

    # Create an HTML file with embedded plots, header, and labels
    create_html_with_plots(plot_images)

    # Return the list of files where the last flux point is above 0.2
    print("Files with last flux point above 0.2:")
    for file_path in files_above_threshold:
        print(file_path)
```

In this updated code:

- A horizontal line at 0.2 is added to each plot.
- The HTML title is changed to red if any of the last 7 points are above the line.
- A list of files where the last flux point is above 0.2 is returned to the main function and printed.


#### You:
ok all the plots have red labels, I only want a check to see if the object is in the files above threshold list and give the HTML a red title if that condition is met

#### ChatGPT:
I see the requirement now. You want to give the HTML title a red color only if the object is in the `files_above_threshold` list. Here's the updated code to achieve that:

```python
import os
import requests
import numpy as np
import matplotlib.pyplot as plt
from astropy.io import fits
from io import BytesIO
import base64

def download_files(link_file_path='download.txt', subfolder='data'):
    # Create the subfolder if it doesn't exist
    if not os.path.exists(subfolder):
        os.mkdir(subfolder)

    # Read the list of links from a text file (one link per line)
    with open(link_file_path, 'r') as file:
        links = file.read().splitlines()

    # Initialize an empty list to store the downloaded file names
    downloaded_files = []

    for link in links:
        response = requests.get(link)
        if response.status_code == 200:
            file_name = os.path.join(subfolder, os.path.basename(link))
            with open(file_name, 'wb') as file:
                file.write(response.content)
            downloaded_files.append(file_name)
        else:
            print(f"Failed to download: {link}")

    return downloaded_files

def check_last_flux_above_threshold(rate, threshold=0.2, last_n_points=7):
    return any(rate[-last_n_points:] > threshold)

def plot_light_curves(downloaded_files):
    plot_images = []
    files_above_threshold = []

    for i, file_path in enumerate(downloaded_files):
        light_curve = fits.open(file_path)
        data = light_curve[1].data
        time = data['TIME']
        rate = data['RATE']
        error = data['ERROR']

        # Determine if the last 7 points are above the threshold
        last_points_above_threshold = check_last_flux_above_threshold(rate)

        plt.figure(i + 1)
        plt.errorbar(time, rate, yerr=error, fmt='o', markersize=2, alpha=0.8, label=f'{os.path.splitext(os.path.basename(file_path))[0]}', color=f'C{i%10}')
        plt.axhline(y=0.2, color='red', linestyle='--')
        plt.xlabel('Time (MJD)')
        plt.ylabel(r'Swift-BAT (15-50 keV) [Counts/cm$^2$/s]')
        plot_title = f'Light Curve {os.path.splitext(os.path.basename(file_path))[0]}'
        if last_points_above_threshold:
            plot_title += ' (Above Threshold)'
        plt.title(plot_title)
        plt.legend()

        # Convert the plot to a base64-encoded image
        buffer = BytesIO()
        plt.savefig(buffer, format='png')
        buffer.seek(0)
        image_data = base64.b64encode(buffer.read()).decode()
        plot_images.append(image_data)

        plt.close()

        if last_points_above_threshold:
            files_above_threshold.append(file_path)

    return plot_images, files_above_threshold

def create_html_with_plots(plot_images, output_file='plots.html', object_name=None):
    with open(output_file, 'w') as html_file:
        html_file.write('<html><head><title>LMXB Trigger for VERITAS</title></head><body>\n')
        html_file.write('<h1>LMXB Trigger for VERITAS</h1>\n')
        html_file.write('<p>This website shows the Swift-BAT daily light curves for the LMXBs visible to VERITAS. If a source is triggered, then we will observe the source with the ECM.</p>\n')

        for i, image_data in enumerate(plot_images):
            if i % 2 == 0:
                html_file.write('<div style="display: flex; flex-direction: row;">\n')
            html_file.write('<div style="margin: 10px;">\n')
            html_file.write(f'<h2 style="color: red;">{os.path.splitext(os.path.basename(downloaded_files[i]))[0]}</h2>\n' if downloaded_files[i] in object_name else f'<h2>{os.path.splitext(os.path.basename(downloaded_files[i]))[0]}</h2>\n')
            html_file.write(f'<img src="data:image/png;base64, {image_data}" />\n')
            html_file.write('</div>\n')
            if i % 2 == 1 or i == len(plot_images) - 1:
                html_file.write('</div>\n')

        html_file.write('</body></html>')

if __name__ == "__main__":
    subfolder = 'data'

    # Download the files and get a list of downloaded file paths
    downloaded_files = download_files(subfolder=subfolder)

    # Plot the light curves and get a list of base64-encoded plot images and files above threshold
    plot_images, files_above_threshold = plot_light_curves(downloaded_files)

    # Create an HTML file with embedded plots, header, and labels
    create_html_with_plots(plot_images, object_name=files_above_threshold)

    # Return the list of files where the last flux point is above 0.2
    print("Files with last flux point above 0.2:")
    for file_path in files_above_threshold:
        print(file_path)
```

In this code, the HTML title is given a red color only if the object is in the `files_above_threshold` list. The `object_name` argument is used to determine if the title should be red or not.


#### You:
Great, 

Now if there are any sources where the last flux is above threshold then can you send out an email using something like the following function

def email(now,c,time,v,dist):
    me='veritasfrb@gmail.com'
    family=['matthew.lundy@mail.mcgill.ca','ragan@physics.mcgill.ca']
    #family=['matthew.lundy@mail.mcgill.ca']

    message= MIMEMultipart('alternative')
    message["Subject"]= 'FRB ALERT'
    message['From'] = me
    message['To']=', '.join(family)
    text = f"""\
        There is an LMXB above treshold! 
        """
    part1 = MIMEText(text, "plain")
    message.attach(part1)
    port= 465
    smtp_server='smtp.gmail.com'
    password='INTENTIONALLY LEFT BLANK'
    context=ssl.create_default_context()
    with smtplib.SMTP_SSL(smtp_server,port,context=context) as server:
        server.login('veritasfrb@gmail.com',password)
        server.sendmail(me,family,message.as_string())



except adding in a section in the text that include which sources are above threshold

#### ChatGPT:
To send an email with information about the sources that are above the threshold, you can modify your code as follows:

```python
import os
import requests
import numpy as np
import matplotlib.pyplot as plt
from astropy.io import fits
from io import BytesIO
import base64
import smtplib
import ssl
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

def download_files(link_file_path='download.txt', subfolder='data'):
    # Create the subfolder if it doesn't exist
    if not os.path.exists(subfolder):
        os.mkdir(subfolder)

    # Read the list of links from a text file (one link per line)
    with open(link_file_path, 'r') as file:
        links = file.read().splitlines()

    # Initialize an empty list to store the downloaded file names
    downloaded_files = []

    for link in links:
        response = requests.get(link)
        if response.status_code == 200:
            file_name = os.path.join(subfolder, os.path.basename(link))
            with open(file_name, 'wb') as file:
                file.write(response.content)
            downloaded_files.append(file_name)
        else:
            print(f"Failed to download: {link}")

    return downloaded_files

def check_last_flux_above_threshold(rate, threshold=0.2, last_n_points=7):
    return any(rate[-last_n_points:] > threshold)

def plot_light_curves(downloaded_files):
    plot_images = []
    files_above_threshold = []

    for i, file_path in enumerate(downloaded_files):
        light_curve = fits.open(file_path)
        data = light_curve[1].data
        time = data['TIME']
        rate = data['RATE']
        error = data['ERROR']

        # Determine if the last 7 points are above the threshold
        last_points_above_threshold = check_last_flux_above_threshold(rate)

        plt.figure(i + 1)
        plt.errorbar(time, rate, yerr=error, fmt='o', markersize=2, alpha=0.8, label=f'{os.path.splitext(os.path.basename(file_path))[0]}', color=f'C{i%10}')
        plt.axhline(y=0.2, color='red', linestyle='--')
        plt.xlabel('Time (MJD)')
        plt.ylabel(r'Swift-BAT (15-50 keV) [Counts/cm$^2$/s]')
        plot_title = f'Light Curve {os.path.splitext(os.path.basename(file_path))[0]}'
        if last_points_above_threshold:
            plot_title += ' (Above Threshold)'
            files_above_threshold.append(os.path.splitext(os.path.basename(file_path))[0])
        plt.title(plot_title)
        plt.legend()

        # Convert the plot to a base64-encoded image
        buffer = BytesIO()
        plt.savefig(buffer, format='png')
        buffer.seek(0)
        image_data = base64.b64encode(buffer.read()).decode()
        plot_images.append(image_data)

        plt.close()

    return plot_images, files_above_threshold

def create_html_with_plots(plot_images, output_file='plots.html', object_name=None):
    with open(output_file, 'w') as html_file:
        html_file.write('<html><head><title>LMXB Trigger for VERITAS</title></head><body>\n')
        html_file.write('<h1>LMXB Trigger for VERITAS</h1>\n')
        html_file.write('<p>This website shows the Swift-BAT daily light curves for the LMXBs visible to VERITAS. If a source is triggered, then we will observe the source with the ECM.</p>\n')

        for i, image_data in enumerate(plot_images):
            if i % 2 == 0:
                html_file.write('<div style="display: flex; flex-direction: row;">\n')
            html_file.write('<div style="margin: 10px;">\n')
            html_file.write(f'<h2 style="color: red;">{os.path.splitext(os.path.basename(downloaded_files[i]))[0]}</h2>\n' if downloaded_files[i] in object_name else f'<h2>{os.path.splitext(os.path.basename(downloaded_files[i]))[0]}</h2>\n')
            html_file.write(f'<img src="data:image/png;base64, {image_data}" />\n')
            html_file.write('</div>\n')
            if i % 2 == 1 or i == len(plot_images) - 1:
                html_file.write('</div>\n')

        html_file.write('</body></html>')

def send_alert_email(files_above_threshold):
    me = 'veritasfrb@gmail.com'
    family = ['matthew.lundy@mail.mcgill.ca', 'ragan@physics.mcgill.ca']

    message = MIMEMultipart('alternative')
    message["Subject"] = 'FRB ALERT'
    message['From'] = me
    message['To'] = ', '.join(family)

    text = f"""\
        FRB ALERT: There is an LMXB above threshold!
        
        Sources above threshold:
        {', '.join(files_above_threshold)}
        """
    part1 = MIMEText(text, "plain")
    message.attach(part1)

    port = 465
    smtp_server = 'smtp.gmail.com'
    password = 'INTENTIONALLY LEFT BLANK'
    context = ssl.create_default_context()
    with smtplib.SMTP_SSL(smtp_server, port, context=context) as server:
        server.login('veritasfrb@gmail.com', password)
        server.sendmail(me, family, message.as_string())

if __name__ == "__main__":
    subfolder = 'data'

    # Download the files and get a list of downloaded file paths
    downloaded_files = download_files(subfolder=subfolder)

    # Plot the light curves and get a list of base64-encoded plot images and files above threshold
    plot_images, files_above_threshold = plot_light_curves(downloaded_files)

    # Create an HTML file with embedded plots, header, and labels
    create_html_with_plots(plot_images, object_name=files_above_threshold)

    # Return the list of files where the last flux point is above 0.2
    print("Files with last flux point above 0.2:")
    for file_path in files_above_threshold:
        print(file_path)

    # Send an alert email if there are sources above threshold
    if files_above_threshold:
        send_alert_email(files_above_threshold)
```

In this code, an `send_alert_email` function is added to send an email with a list of sources above the threshold. The email is sent if there are sources in the `files_above_threshold` list. The email includes a list of sources that are above the threshold.


#### You:
Ok great, now I need to setup a seperate script that can execute this script every day. I'm going to run it as a cronjob so make it a .sh script. The only thing this script needs to do is run this python script and then take the plot.html output and scp it onto a host 

#### ChatGPT:
To create a shell script that runs the Python script and then uses `scp` to copy the `plots.html` file to a remote host, you can create a file with a `.sh` extension (e.g., `daily_script.sh`) and add the following content:

```bash
#!/bin/bash

# Set the Python script filename
python_script="your_python_script.py"  # Replace with the actual filename of your Python script

# Run the Python script
python3 "$python_script"

# Check if the Python script generated "plots.html"
if [ -f "plots.html" ]; then
    # Set the remote host and destination directory for SCP
    remote_host="your_remote_host"
    remote_directory="your_remote_directory"  # Replace with the actual destination directory

    # Use SCP to copy "plots.html" to the remote host
    scp "plots.html" "${remote_host}:${remote_directory}"
    echo "plots.html copied to ${remote_host}:${remote_directory}"
else
    echo "plots.html not found. Ensure that the Python script generates it."
fi
```

Make sure to replace `"your_python_script.py"` with the actual filename of your Python script and set the remote host and destination directory as needed. This shell script will run the Python script and then copy the `plots.html` file to the specified remote host and directory.

Once you've created the shell script, you can set it up as a cron job to run it daily by adding an entry to your crontab. To edit your crontab, you can use the `crontab -e` command, and then add the following line to run the shell script at a specific time daily:

```
0 0 * * * /bin/bash /path/to/daily_script.sh
```

This line schedules the script to run at midnight every day. Don't forget to replace `/path/to/daily_script.sh` with the actual path to your shell script. Save and exit your crontab editor to activate the cron job.


#### You:
ok crontab is not running the script properly the script is located in /home/matthew/LMXB/cron.sh and I've changed the /bin/bash to source and added a line to cd into the directory but it still doesn't work

#### ChatGPT:
If you're encountering issues with your cron job not running the script properly, it's important to ensure that the environment is set up correctly and that you have specified the full paths to your script and any required files. Here are some steps you can take to troubleshoot and potentially fix the issue:

1. **Specify Full Paths**: In your cron job entry, specify the full paths for both the shell script and the Python script to avoid any relative path issues. For example:

   ```bash
   0 0 * * * /bin/bash /home/matthew/LMXB/cron.sh
   ```

2. **Set Environment Variables**: If your Python script relies on specific environment variables, make sure they are set correctly in the cron job. You can set environment variables within your shell script using the `export` command.

3. **Check Permissions**: Ensure that the script and any files it accesses have the appropriate permissions. You may need to make the script executable using `chmod +x /home/matthew/LMXB/cron.sh`.

4. **Redirect Output**: It's a good practice to redirect the standard output and standard error to a file within your script or in your cron job entry. This can help you diagnose any issues. For example:

   ```bash
   0 0 * * * /bin/bash /home/matthew/LMXB/cron.sh > /home/matthew/cron_output.log 2>&1
   ```

5. **Working Directory**: If your script relies on a specific working directory, make sure to `cd` to that directory in your shell script. Use absolute paths to access any files or directories within the script.

6. **Errors in Script**: Double-check for errors or exceptions in your Python script that might prevent it from running successfully. Ensure that all dependencies and libraries are available in the environment where the script runs.

7. **User Permissions**: Ensure that the user running the cron job has the necessary permissions to execute the script and access any required resources.

8. **Cron Daemon Logs**: Check system logs for any error messages related to the cron job. On many systems, you can view the cron daemon logs in `/var/log/cron` or `/var/log/syslog`.

By following these steps and checking for potential issues, you should be able to identify and resolve the problem with your cron job.
