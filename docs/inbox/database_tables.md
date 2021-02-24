# Database Tables

## Files
| column             | required | type          | description               | default                      |
| ----------------   | -------- | ------------- | --------------------      | ----------------             |
| id                 | true     | int           | Primary key               | autoincrement_pk             |
| uuid               | true     | uuid4         | Universally unique ID     | uuid4                        |
| file               | true     | nvarchar(max) | Original file URI         |                              |
| size               | true     | int           | File size in bytes        |                              |
| puid               | true     | nvarchar(20)  | Original file PUID        |                              |
| checksum           | true     | nvarchar(max) | File checksum             |                              |

## Locations
| column             | required | type          | description               | default                      |
| ----------------   | -------- | ------------- | --------------------      | ----------------             |
| id                 | true     | int           | Primary key               | autoincrement_pk             |
| file_id            | true     | int           | Foreign key (Files.id)    | Files.id                     |
| location           | true     | nvarchar(max) | File location URI         |                              |
| PUID               | true     | nvarchar(20)  | PUID for file at location |                              |
| type               | true     | nvarchar(50)  | Location type             | E.g. main archive, statutory |

## Conversion
| column             | required | type         | description            | default          |
| ------------------ | -------- | ------------ | --------------------   | ---------------- |
| id                 | true     | int          | Primary key            | autoincrement_pk |
| puid               | true     | nvarchar(20) | PUID to convert from   |                  |
| archival_puid      | true     | nvarchar(20) | Main ACA archival PUID |                  |
| statutory_puid     | false    | nvarchar(20) | Statutory PUID         | fmt/353 (TIFF)   |
