File formats can be complex, especially for large volumes of long term data storage.
To support all your custom formats, we have a Data Preprocessor for all your machines. 
This Preprocessor gives you full freedom to import any type of data, from CAN, to AVRO, 
to Parquet or HDF. You find this Preprocessor in `Integration` > `Machine Data`. 

Off the shelf, we support the following file formats:

- Comma Separated Values (.csv)
- Javascript Object Notation (.json)
- Extensible Markup Language (.xml)
- Feather (.feather)
- Parquet (.parquet)
- State (.stata)
- Pickle (.pickle)


## Custom File Support
On the platform, every machine has a unique preprocessor. You can edit this preprocessor to suit your needs. 
For example, you can support AVRO or CAN data, extract serial numbers or other information. The preprocessor is 
written in Python 3.9. The DataFrame is parsed with Pandas. 

### Flags & Serial Number
If the serial number is available in the data, it is beneficial to extract that during preprocessing. 
You can do so by adding the `serial` field to the flags. This will naturally be ingested so that you can find your 
alerts, diagnoses and logs easier. 
```python
import pandas as pd
from io import BytesIO


class Preprocessor:
    
    def __init__(self):
        self.version = "v1"
        self.supported = [".csv"]
        self.machine = "ENTER MACHINE NAME"
    
    def process(self, raw_file, **kwargs):
        # Parse raw file
        file_string = raw_file.read()
        file_bytes = BytesIO(file_string)
        
        # Get Dataframe
        df = pd.read_csv(file_bytes)
        
        # Extract Serial
        serial = df['serial'].iloc[0]
        
        # Return Dataframe & serial
        return {"df": df, "flags": {"serial": serial}}
```
> **_NOTE:_** 
    Similar to extracting the serial number, other flags can be extracted and visualized on request.

### CAN Support

To support CAN data, upload your DBC file as a dependency. 
Go to `Integration` > `Machine Data` and click `edit` under the `Preprocessor`. 
Copy the code below into the code editor and send it for review.
Note that you have to adept:

1. Your Machine Name
2. ID Key
3. Data Key
4. Date Key
5. Flags

```python
import logging
import pandas as pd
from io import BytesIO

class Preprocessor:

    def __init__(self):
        self.version = "v1"
        self.supported = [".csv", ".json", ".xml", ".feather", ".parquet", ".stata", ".pickle"]
        self.machine = "ENTER MACHINE NAME"

    def process(self, raw_file, **kwargs):
        """
        Preprocesses a CAN encoded measurement log.
        """
        logging.info(f"Preprocessing {raw_file.name}")
        f_format = raw_file.name[raw_file.name.rfind('.'):].lower()
        assert f_format in self.supported, "File format not supported"
        assert 'dbc' in kwargs, "CAN Database Missing"

        # Read file
        file_string = raw_file.read()
        string_io = BytesIO(file_string)
        if f_format == ".csv":
            df = pd.read_csv(string_io, sep=',', index_col=False)
        elif f_format == ".json":
            df = pd.read_json(string_io)
        elif f_format == ".xml":
            df = pd.read_xml(string_io)
        elif f_format == ".feather":
            df = pd.read_feather(string_io)
        elif f_format == ".parquet":
            df = pd.read_parquet(string_io)
        elif f_format == ".pickle":
            df = pd.read_pickle(string_io)
        else:
            raise NotImplementedError

        # Parse CAN data
        dbc_db = kwargs["dbc"]

        # Iterate through messages
        df_list = []
        for i in range(len(df)):
            try:
                # Get row
                row = df.iloc[i]
                can_id = row["ENTER ID KEY"]
                can_bytes = row["ENTER DATA KEY"]
                decoded = dbc_db.decode_message(can_id, can_bytes)
                decoded['ts'] = row["ENTER DATE KEY"]
            except Exception as e:
                logging.exception(e)
                pass
        df = pd.DataFrame(df_list)

        return {'df': df, 'flags': {}}
```

### AVRO Support
            
Similar to CAN support, AVRO requires a schema like the one below:

```json
{
    "type": "record",
    "namespace": "Example",
    "name": "Entity",
    "fields": [
        {"name": "id", "type": "int"},
        {"name": "ts", "type": "str"}
    ]
}
```



To support such encodings, copy the following code into the preprocessor:

```python
import logging
import pandas as pd
from io import BytesIO
from fastavro import parse_schema
from fastavro import schemaless_reader


class Preprocessor:

    def __init__(self):
        self.version = "v1"
        self.supported = [".avro"]
        self.machine = "ENTER MACHINE NAME"

def process(self, raw_file, **kwargs):
    """
    Preprocesses an AVRO encoded measurement log.
    """
    logging.info(f"Preprocessing {raw_file.name}")
    f_format = raw_file.name[raw_file.name.rfind('.'):].lower()
    assert f_format in self.supported, "File format not supported"
    assert 'json' in kwargs, "AVRO Schema"

    # Schema
    schema = parse_schema(kwargs['json'])

    # Convert
    file_string = raw_file.read()
    file_bytes = BytesIO(file_string)
    data = schemaless_reader(file_bytes, schema)
    df = pd.DataFrame(data)

    return {'df': df, 'flags': {}}
```
