# ETD Loader Process

It is stored inside IR project.
GitHub: https://github.com/culibraries/ir-scholar/tree/master/etd-loader

## Folder Structure

All the scripts running are stored inside IR container. However, all of the ETD files are store at: /efs/prod/proquest/ or /efs/test/proquest.
Access scholar-worker image with Kubectl. Then go to <code>/efs/prod/proquest</code> or <code>/efs/test/proquest</code> to see those files.

* .zip: new zip files, have not processed.
* .zip.proccessed: files have been unzipped and processed
* logs/ : folder to store log files
* proccessing_folder/: folder to store files after unzip (.zip files) to process.
* rejected/: folder to store rejected files(.zip). If there is an error happen during the process. The script will move .zip error file to this folder.
* unaccepted/: folder to store unaccepted files(.zip). If the ETD item is not allow to load to IR. It will be moved to this folder.

## Execute Script

* Step 1: scholar-worker
    ```sh
    kubectl exec -it scholar-worker-78f7c8646-mztqv -n scholar -- bash
    ```
* Step 2: at /app run command below:

    In TEST:

    <code>python3 etd-loader/main.py /efs/test/proquest/ /efs/test/proquest/processing_folder/ number_item</code>

    In PRODUCTION:

    <code>python3 etd-loader/main.py /efs/prod/proquest/ /efs/prod/proquest/processing_folder/ number_item</code>

    * number_item: is number of zip file that you want to process. You can put any number as long as it is easy to keep track. Less than 20 is the suggestion.
    * etd-loader/main.py: access to script.
    * /efs/test/proquest/: where the ETD .zip files store.
    * /efs/prod/proquest/processing_folder/: where ETD .zip files extract.


* Step 3: Go to scholar website to make sure its loaded including any upload files. Check logs and folders to see if is there any rejected files, unaccepted files or any error might happen.