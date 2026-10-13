# Cloud Transfer Notebook

A self-contained Google Colab notebook for copying public files and folders from
Yandex Disk to Google Drive.

Uses the official [Yandex Disk REST API](https://yandex.ru/dev/disk/rest/) to list
public resources and request download links.

<center>
<img src="./assets/image.png" width="900">
</center>

## Usage

1. Download [the notebook](migration.ipynb) and open it through **File → Upload notebook** at [Google Colab](https://colab.research.google.com/).
2. Run the first cell to install the dependencies. Use a CPU runtime; no GPU is needed.
3. Run the Google Drive mounting cell and grant access to the account that should receive the files.
4. In the settings cell, choose a destination and add your public links:

   ```python
   destination = "/content/drive/MyDrive/cloud-import"
   yandex_links = {
       "Files": [
           "https://disk.yandex.ru/i/FIRST_PUBLIC_KEY",
           "https://disk.yandex.ru/d/SECOND_PUBLIC_KEY",
       ],
       "Archive": "https://disk.yandex.ru/d/ANOTHER_PUBLIC_KEY",
   }
   overwrite = False
   ```

5. Run the remaining cells to load the transfer code and start copying.

Replace the example links with your own. Each dictionary key names a subdirectory
in the destination. Use a list of links to copy several resources into the same
subdirectory; a single link still works. Do not repeat dictionary keys: Python
keeps only the last value. The example above copies both links into
`MyDrive/cloud-import/Files`. Links to individual files and nested folders are
supported. The notebook contains all the code it needs.

Files are downloaded to the runtime's local disk in 1 MiB chunks, verified, and
then copied to Google Drive. Allow enough free local space for one file. Nested
folders and original filenames are preserved. Original files remain on Yandex
Disk. Existing copies are verified and skipped when they match; set
`overwrite = True` to replace other existing files deliberately. Different files
with the same destination name produce an error when overwriting is disabled.
The final report shows downloaded files, verified files skipped, and errors.

## Retrying transfers

Downloads retry up to eight times, with a 30-second connection timeout and a
300-second read timeout. The read timeout limits waiting for data, not the total
download duration. A fresh download URL is requested for each attempt.

Partial downloads remain on the runtime's local disk. Rerun the copying cell in
the same runtime to resume them when the server supports range requests. If a
resume request is ignored, the file is downloaded from the beginning. Invalid
range responses or failed integrity checks also restart the download.

Resetting or deleting the Colab runtime removes its local partial files. Files
already copied to Google Drive remain available. If writing to Drive fails, the
verified local download is retained so the copying cell can retry without
downloading it again.

## License

[MIT](LICENSE)
