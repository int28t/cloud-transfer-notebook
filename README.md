# Cloud Transfer Notebook

A self-contained Google Colab notebook for copying public files and folders from
Yandex Disk to Google Drive.

Uses the official [Yandex Disk REST API](https://yandex.ru/dev/disk/rest/) to list
public resources and request download links.

## Usage

1. Download [the notebook](migration.ipynb) and open it through **File → Upload notebook** at [Google Colab](https://colab.research.google.com/).
2. Run the first cell to install the dependencies. Use a CPU runtime; no GPU is needed.
3. Run the Google Drive mounting cell and grant access to the account that should receive the files.
4. In the settings cell, choose a destination and add your public links:

   ```python
   destination = "/content/drive/MyDrive/cloud-import"
   yandex_links = {
       "Documents": "https://disk.yandex.ru/d/YOUR_PUBLIC_KEY",
   }
   overwrite = False
   ```

5. Run the remaining cells to load the transfer code and start copying.

Replace the example link with your own. You can add multiple links. Each dictionary
key names a subdirectory in the destination: the example above copies files into
`MyDrive/cloud-import/Documents`. Links to individual files and nested folders
are supported. The notebook contains all the code it needs.

Files are streamed in 1 MiB chunks, and nested folders are preserved. Original
files remain on Yandex Disk. Existing copies are verified and skipped when they
match; set `overwrite = True` to replace other existing files deliberately.
The final report shows downloaded files, verified files skipped, and errors.

## License

[MIT](LICENSE)
