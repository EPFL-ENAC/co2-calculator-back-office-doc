# Data Management Guide

!!! warning "Under construction"
This section is under development. So the content may be incomplete and subject to change.

This guide provides comprehensive data validation schemas for all modules in the CO2 Calculator. These specifications help data managers prepare and upload data correctly.

---

## General Notes

!!! info "Data and factor files" - module_data.csv: data uploaded by the backmanagement office. Here it is important to always have the info of the unit_institutional_id. - factors.csv: factors that are necessary for the calculation. (emission factors and/or other factors) - module_template.csv: file that has the names of the columns, and a line of example. Users can download it by clicking on "Download CSV Template". - module_test.csv: file that is uploaded via the button "Upload CSV". In this CSV, we have the same columns as in the CSV "template", (and there is no unit_institutional_id since the user is already inside the unit).

!!! info "Important Information" - **unit_institutional_id**: For institutional data where relevant, this field contains the unit identifier (e.g., for EPFL: cf_id as 4-digit numbers). - **kg_co2eq**: When this optional field is provided with a value, no calculation is performed for that line - the value is used directly. - **note**: Available across all modules to add any relevant comments or explanations for specific entries. - **Template and Test files**: Used for data entry by unit managers, while data files are typically pre-filled by the back-office. - **Factors files**: Contain emission factors and conversion coefficients required for calculations.

!!! warning "Data Validation" - Order of upload from the configuration back-office: the factors.csv file needs to be uploaded **before** the data.csv file, as certain requirements for data depend on what is given in the factors. - Rows that don't meet mandatory field requirements or value constraints will be ignored during upload. - Warning messages will be displayed when data doesn't match factor files. - Date formats must follow ISO standard (YYYY-MM-DD).

---

## Headcount

???+ info "headcount_data.csv"

    | field | type | mandatory | values constraints | description |
    |-------|------|-----------|-------------------|-------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits numbers only) |
    | name | string | ✅ | non-empty string | e.g. First and second name |
    | sius_code | string | ✅ | within `51, 52, 53, 54, 56, 57, 58, 59` | SIUS function code. Used to build the FTE-per-function chart. |
    | user_institutional_id | string | ✅ | numbers only | for EPFL: sciper |
    | fte | float | ✅ | 0 ≤ float ≤ 1 | Full-time equivalent e.g. 1.0, 0.8, 0.05, 0.75 |
    | note | string | ❌ | - | contains the note if needed |

    !!! note
        `fte` can be completed directly in the table if not provided in the file, and must be filled to validate the module.

    ??? info "SIUS code reference"

        | Code | FR | EN |
        |------|----|----|
        | 51 | Enseignant·e·s habilité·e·s à diriger une unité organisationnelle | Professors |
        | 52 | Autres enseignant·e·s | Other teaching staff |
        | 53 | Collaborateur·trices scientifiques | Scientific collaborators |
        | 54 | Assistant·e·s et/ou doctorant·e·s | Scientific and doctoral assistants |
        | 56 | Personnel de direction de la haute école | Managerial staff |
        | 57 | Personnel administratif | Administrative staff |
        | 58 | Personnel de soutien | Support staff |
        | 59 | Personnel d'exploitation | Operational staff |

???+ info "headcount_template.csv and headcount_test.csv"

    | field | type | mandatory | values constraints | description |
    |-------|------|-----------|-------------------|-------------|
    | name | string | ✅ | non-empty string | e.g. First and second name |
    | sius_code | string | ✅ | within `51, 52, 53, 54, 56, 57, 58, 59` | SIUS function code. Used to build the FTE-per-function chart. |
    | user_institutional_id | string | ✅ | numbers only | for EPFL: sciper |
    | fte | float | ✅ | 0 ≤ float ≤ 1 | Full-time equivalent e.g. 1.0, 0.8 |
    | note | string | ❌ | - | contains the note if needed |

    !!! note
        no test and template for this module - table to delete.

---

## Process Emissions

???+ info "processemissions_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits, numbers only) |
    | category | string | ✅ | within `processes_factors.csv` | e.g. Refrigerant |
    | subcategory | string | ❌ | within `processes_factors.csv`, can be `None`. | If the category is `Refrigerants` the subcategory needs to be specified to choose the corresponding factor.   e.g. R145 |
    | quantity | float | ✅ | in kg, float >=0 | e.g. 34 |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |

???+ info "processemissions_test.csv and processemissions_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | category | string | ✅ | within `processes_factors.csv` | e.g. Refrigerant |
    | subcategory | string |❌ | within `processes_factors.csv`, can be `None`. | If the category is `Refrigerants` the subcategory needs to be specified to choose the corresponding factor.   e.g. R145 |
    | quantity | float | ✅ | in kg, float >=0 | e.g. 34 |
    | note | string | ❌ | - | contains the note if needed |

???+ info "processemissions_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | category | string | ✅ | - | for EPFL: Refrigerants, CH4, N2O, CO2 |
    | subcategory | string | ❌ | - | If the category is `Refrigerants` the subcategory is always specified in the factors used at EPFL. |
    | unit | string | ✅ | - | eg kg |
    | ef_kg_co2eq_per_unit | float | ✅ | 0 ≤ float | e.g. 23'500 (kg CO2 eq / kg for SF6) |
    
---
## Buildings

???+ info "building_energycombustions_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits, numbers only) |
    | name | string | ✅ | within `building_combustions_factors.csv` | e.g. "natural_gas" |
    | unit | string | ✅ | in SI format | eg kg, kWh , if couple "unit" "name" not found in `building_energycombustions_factors.csv` row ignore with warning message |
    | quantity | float | ✅ | 0 ≤ float | e.g. 34 |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |

???+ info "building_energycombustions_test.csv & building_energycombustions_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | name | string | ✅ | within `building_combustions_factors.csv` | e.g. "natural_gas" |
    | unit | string | ✅ | in SI format | eg kg, kWh , if couple "unit" "name" not found in `building_energycombustions_factors.csv` row ignore with warning message |
    | quantity | float | ✅ | 0 ≤ float | e.g. 34 |
    | note | string | ❌ | - | contains the note if needed |

???+ info "building_energycombustions_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit | string | ✅ | in SI format | e.g. kWh, kg |
    | name | string | ✅ | has to be unique, see table below | e.g. natural_gas |
    | ef_kg_co2eq_per_unit | float | ✅ | 0 ≤ float | e.g. 0.05 |
    
    ??? info "name reference table"

        | name | FR | EN |
        |------|----|----|
        | natural_gas | Gaz naturel | Natural gas |
        | heating_oil | Mazout | Heating oil |
        | biomethane | Biométhane | Biomethane |
        | propane | Propane | Propane |
        | pellets | Granulés de bois | Pellets |
        | forest_chips | Plaquettes forestières | Forest chips |
        | wood_logs | Bois bûche | Wood logs |

        
???+ info "building_rooms_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits, numbers only) |
    | building_name | string | ✅ | within `building_rooms_factors.csv` | e.g. GC |
    | room_name | string | ✅ | i.e. AI0122 | If the correspondence (building_name, room_name) is not found in the reference, the row is ignored with a warning message (we do not have the info on squared meters without the name, and so we cannot do the calculation). |
    | room_type | string | ✅ | within `office, miscellaneous, laboratories, archives, libraries, auditoriums`, see table below | e.g. "office". it can be modified in the table by the user. If different from buildings_room_reference.csv it overwrites it. This is the information that is used for the calculation.  |
    | room_allocation_ratio | float | ❌ | 0 ≤ float ≤ 1.0 | Describe the allocation of a room for a sinle unit, in case of shared rooms etc... e.g. 0.8. If not given, default to 1 |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line. For EPFL, this must be filled for SCITAS, RCP, etc |

???+ info "building_rooms_test.csv and building_rooms_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | building_location | string | ❌ | can be `None` | e.g. "ECUBLENS" |
    | building_name | string | ✅ | within `building_rooms_factors.csv` | e.g. GC |
    | room_name | string | ✅ | i.e. AI0122 | If the correspondence (building_name, room_name) is not found in the reference, the row is ignored with a warning message (we do not have the info on squared meters without the name, and so we cannot do the calculation). |
    | room_type | string | ✅ | within `office, miscellaneous, laboratories, archives, libraries, auditoriums`, see table below | e.g. "office". it can be modified in the table by the user. If different from buildings_room_reference.csv it overwrites it. This is the information that is used for the calculation. If the correspondence building, room_name, room_type does not exist, use the kwh_per_square_meter for the building, room_type (the room_name does not influence the consumption). |
    | note | string | ❌ | - | contains the note if needed |

???+ info "building_rooms_reference.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | building_location | string | ❌ | can be `None` | e.g. "ECUBLENS" |
    | building_name | string | ✅ | within `building_rooms_factors.csv` | e.g. GC |
    | room_name | string | ✅ | digit or name | i.e. AI0122 |
    | room_type | string | ✅ | within `office, miscellaneous, laboratories, archives, libraries, auditoriums`, see table below | The type in this file is the one that is used by default when adding a new line. However, it can be changed by the user, and if changed, the chosen type is used to pair with the corresponding factors in `building_rooms_factors.csv` (only type and building are taken to compute co2_eq). |
    | room_surface_square_meter | float | ✅ | 0 ≤ float | e.g. 12 |

???+ info "building_rooms_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | building_name | string | ✅ | - | for EPFL: BCH,BS... |
    | room_type | string | ✅ | within `office, miscellaneous, laboratories, archives, libraries, auditoriums`, see table below | e.g. "office"  |
    | heating_kwh_per_square_meter | float | ✅ | 0 ≤ float | e.g. 2.3. These are the consumption hypotheses in squared meters for the given building and type of room. This column gives the hypotheses for all buildings, which are used for the calculation also when the data are input by the users (via the upload .csv)  |
    | cooling_kwh_per_square_meter | float | ✅ | 0 ≤ float | e.g. 2.3. These are the consumption hypotheses in squared meters for the given building and type of room. This column gives the hypotheses for all buildings, which are used for the calculation also when the data are input by the users (via the upload .csv)  |
    | ventilation_kwh_per_square_meter | float | ✅ | 0 ≤ float | e.g. 2.3. These are the consumption hypotheses in squared meters for the given building and type of room. This column gives the hypotheses for all buildings, which are used for the calculation also when the data are input by the users (via the upload .csv)  |
    | lighting_kwh_per_square_meter | float | ✅ | 0 ≤ float | e.g. 2.3. These are the consumption hypotheses in squared meters for the given building and type of room. This column gives the hypotheses for all buildings, which are used for the calculation also when the data are input by the users (via the upload .csv)  |
    | ef_kg_co2eq_per_kwh | float | ✅ | 0 ≤ float | e.g. 0.125 |
    | energy_type | string | ✅ | `electric, thermal, etc` | e.g. "electric" |
    | conversion_factor | float | ❌ | can be `None` | e.g. 4 , if `None` consider as 1. This is an adjustement factor needed because in some cases the kwh are elec, in other thermique, etc. And we need to bring it to right type to use the emission factors properly.  |


    ??? info "room_type reference table"

        | name | FR | EN |
        |------|----|----|
        | office | Bureau | Office |
        | miscellaneous | Divers | Miscellaneous |
        | laboratories | Laboratoires | Laboratories |
        | archives | Archives | Archives |
        | libraries | Bibliothèques | Libraries |
        | auditoriums | Auditoires | Auditoriums |
---


## Equipment

???+ info "equipment_data.csv"

    | field | type | mandatory | values constraints | description |
    |-------|------|-----------|-------------------|-------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits numbers only) |
    | equipment_id | string | ✅ | letters and numbers | This info is used to deal with the updates between one year and the other, in particular for usage hours information. for EPFL: Inventory number |
    | name | string | ✅ | non-empty string | e.g. name of the equipment "GoPro" |
    | equipment_class | string  | ✅ | within `equipments_factors.csv` | e.g. "Evaporator" is used to get power in `equipments_factors.csv`, for EPFL these are the standard inventory classes in ENG. If `equipment_class` is not empty but the value is not listed to `equipments_factors.csv`, the row is ignored. A warning message is displayed in loading. |
    | sub_class | string  | ❌ | within `equipments_factors.csv`, can be `None` | e.g. "Vacuum evaporators". The sub_class is filled by the user. Must be in the tuples equipment_class/sub_class within `equipments_factors.csv` else row ignored - (Warning message in loading if data uploaded with the wrong subclass) |
    | active_usage_hours_per_week | int | ❌ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | standby_usage_hours_per_week | int | ❌ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given, no calculation is performed on this line |

???+ info "equipment_template.csv and equipment_test.csv"

    | field | type | mandatory | values constraints | description |
    |-------|------|-----------|-------------------|-------------|
    | equipment_id | string | ✅ | letters and numbers | This info is used to deal with the updates between one year and the other, in particular for usage hours information. for EPFL: Inventory number |
    | name | string | ✅ | non-empty string | e.g. name of the equipment "GoPro" |
    | equipment_class | string  | ✅ | within `equipments_factors.csv` | e.g. "Evaporator" is used to get power in `equipments_factors.csv`, for EPFL these are the standard inventory classes in ENG. If `equipment_class` is not empty but the value is not listed to `equipments_factors.csv`, the row is ignored. A warning message is displayed in loading. |
    | sub_class | string  | ❌ | within `equipments_factors.csv`, can be `None` |  e.g. "Vacuum evaporators". The sub_class is filled by the user. Must be in the tuples equipment_class/sub_class within `equipments_factors.csv` else row ignored - (Warning message in loading if data uploaded with the wrong subclass)|
    | active_usage_hours_per_week | int | ❌ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | standby_usage_hours_per_week | int | ❌ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | note | string | ❌ | - | contains the note if needed |

???+ info "equipment_factors.csv"

    | field | type | mandatory | values constraints | description |
    |-------|------|-----------|-------------------|-------------|
    | equipment_category | string | ✅ | non-empty string within : {`scientific,it,other`} | e.g. scientific. Case-sensitive |
    | equipment_class | string  | ✅ | not empty string | e.g. "Evaporator". For EPFL these are the standard inventory classes in ENG |
    | sub_class | string | ❌ | can be `None` | e.g. "Vacuum evaporators" |
    | active_usage_hours_per_week | int | ✅ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | standby_usage_hours_per_week | int | ✅ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | active_power_w | float | ✅ | ≥ 0 | e.g. 23 |
    | standby_power_w | float | ✅ | ≥ 0 | e.g. 23 |
    | ef_kg_co2eq_per_kwh | float | ✅ | ≥ 0 | e.g. swiss mix 0.125 |

---

## Purchases

???+ info "purchases_common_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits, numbers only) |
    | name | string | ✅ | non-empty string | e.g. "HP Novobook" |
    | supplier | string  | ❌ | - | e.g. "Bentley Systems International Ltd" |
    | quantity | float | ❌ | 0 ≤ float | e.g. 3 |
    | total_spent_amount | float | ✅ | 0 ≤ float | e.g. 3567 |
    | currency | string | ❌ | in `chf, eur, usd, gbp, aud, etc` format. If not given, chf is used by default. | e.g. eur |
    | purchase_institutional_code | string | ✅ | within `purchases_common_factors.csv` | e.g. UNSPSC code, as to within `purchases_common_factors.csv` |
    | purchase_institutional_description | string | ❌ | - | e.g. UNSPSC description, if not given compute with `purchases_common_factors.csv` |
    | purchase_additional_code | string | ❌ | within `purchases_common_factors.csv` | e.g. NACRES code, this column is used for the co2 emission calculations. For EPFL: data uploaded from the data manager will always have this column filled. But users can add purchases without this code and a static mapping (contained in the factors) is used for UNSPCS -> NACRES. |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |

???+ info "purchases_common_test.csv and purchases_common_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | name | string | ✅ | non-empty string | e.g. "HP Novobook" |
    | supplier | string  | ❌ | - | e.g. "Bentley Systems International Ltd" |
    | quantity | float | ❌ | 0 ≤ float | e.g. 3 |
    | total_spent_amount | float | ✅ | 0 ≤ float | e.g. 3567 |
    | currency | string | ❌ | in `chf, eur, usd, gbp, aud, etc` format. If not given, chf is used by default. | e.g. eur |
    | purchase_institutional_code | string | ✅ | within `purchases_factors.csv` | e.g. UNSPSC code |
    | purchase_institutional_description | string | ❌ | - | e.g. UNSPSC description, if not given compute with `purchases_common_factors.csv` |
    | purchase_additional_code | string | ❌ | within `purchases_factors.csv` | e.g. NACRES code |
    | note | string | ❌ | - | contains the note if needed |

???+ info "purchases_common_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | currency | string | ✅ | in `chf, eur, usd, gbp, aud, etc` format. | for labo1point5 is eur |
    | purchase_category | string | ✅ | within `it_equipment,other,scientific_equipment,services,vehicle,consumable_accessories,biological_chemical_gaseous_product` | e.g. `vehicle`. This columns is used to split the purchases into the subsections in the module. |
    | purchase_institutional_code | string | ✅ | - | e.g. UNSPSC code |
    | translation_key | string | ❌ | key for the translation of the UNSPSC purchase description. If not given, the key is displayed. | e.g. |
    | purchase_additional_code | string | ✅ | - | e.g. NACRES code |
    | ef_kg_co2eq_per_currency | float | ✅ | 0 ≤ float | e.g. 0.1 |

???+ info "purchases_centralized_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-dits, numbers only) |
    | name | string | ✅ | non-empty string | e.g. "Liquid nitrogen" |
    | unit | string | ✅ | - | e.g. liter |
    | annual_consumption | float | ✅ | - | e.g. 45.05 |
    | coef_to_kg | float | ✅ | non negative | e.g. 3.05 |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |

???+ info "purchases_centralized_test.csv and purchases_centralized_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | name | string | ✅ | non-empty string | e.g. "Liquid nitrogen" |
    | unit | string | ✅ | - | e.g. liter |
    | annual_consumption | float | ✅ | - | e.g. 45.05 |
    | coef_to_kg | float | ✅ | non negative | e.g. 3.05 |
    | note | string | ❌ | no constraints | contains the note if needed |

???+ info "purchases_centralized_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | name | string | ✅ | non-empty string | e.g. "Liquid nitrogen" |
    | ef_kg_co2eq_per_kg | float | ✅ | 0 ≤ float | e.g. 0.1 |

---

## External Clouds & AI

???+ info "external_ai_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits numbers only) |
    | provider | string | ✅ | within `external_ai_factors.csv` | e.g. name of the firm "Google" |
    | usage_type | string | ✅ | within `external_ai_factors.csv` | e.g. "text,video,image", tuple provider/usage_type within `external_ai_factors`. If not in provided tuple, the raw is ignored with a Warning message. |
    | requests_per_user_per_day | string |  ✅ | within "1-5 times per day", "5-20 times per day", "20-100 times per day", ">100 times per day" | e.g. 1-5 times per day |
    | fte_count | float | ✅ | 1 ≤ float | e.g. 2. By default the numbers shown is the total headcount of the unit. |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed |

???+ info "external_ai_test.csv and external_ai_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | provider | string | ✅ | have to be in `external_ai_factors.csv` | e.g. name of the firm "Google" |
    | usage_type | string | ✅ | have to be in `external_ai_factors.csv` | e.g. "text,video,image" |
    | requests_per_user_per_day | string | ✅ | within "1-5 times per day", "5-20 times per day", "20-100 times per day", ">100 times per day" | e.g. 1-5 times per day |
    | fte_count | float | ✅ | 1 ≤ float | e.g. 2. By default the numbers shown is the total headcount of the unit. |
    | note | string | ❌ | - | contains the note if needed |

???+ info "external_ai_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | provider | string | ✅ | - | e.g. name of the firm "Google" |
    | usage_type | string | ✅ | - | e.g. type of use "text,video,image" |
    | ef_kg_co2eq_per_request | float | ✅ | 0 ≤ float | e.g. "0.05" |

???+ info "external_clouds_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits numbers only) |
    | service_type | string | ✅ | within `external_clouds_factors.csv` | e.g. one of `storage,compute` |
    | provider | string | ✅ | within `external_clouds_factors.csv` | e.g."AWS" |
    | spent_amount | float | ✅ | 0 ≤ float | e.g. 299 |
    | currency | string | ❌ | in `chf, eur` format. If not given, eur is used by default. | e.g. eur |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed |

???+ info "external_clouds_test.csv and external_clouds_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | service_type | string | ✅ | within `external_clouds_factors.csv` | e.g. one of `storage,compute` |
    | provider | string | ✅ | within `external_clouds_factors.csv` | e.g."AWS" |
    | spent_amount | float | ✅ | 0 ≤ float | e.g. 299 |
    | currency | string | ❌ | in `chf, eur` format. If not given, eur is used by default. | e.g. eur |
    | note | string | ❌ | - | contains the note if need |

???+ info "external_clouds_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | service_type | string | ✅ | not empty | e.g. `storage,compute` |
    | provider | string | ✅ | not empty | e.g. firm |
    | currency | string | ✅ | in `chf, eur` format, not `None`. | for EPFL -> eur |
    | ef_kg_co2eq_per_currency | float | ✅ | 0 ≤ float | e.g. 0.8 |

---

## Travel

???+ info "travel_planes_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (numbers only) |
    | origin_iata | string | ✅ | IATA code | e.g. "GVA". |
    | destination_iata | string | ✅ | IATA code | e.g. "JFK".|
    | user_institutional_id | string | ✅ | - | e.g. EPFL: SCIPER |
    | departure_date | string | ❌ | ISO format YYYY-MM-DD | e.g. "2025-05-15" if date format not recognized ignore row, id date not in the carbon report year ignore row. If None the row is considered.  |
    | number_of_trips | int | ✅ | 1 ≤ int | e.g. 2 |
    | cabin_class | string | ✅ | within `first,business,economy` | e.g. business. For EPFL when taken from API, premium economy needs to be classifed as eco. |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line. |

???+ info "travel_planes_test.csv and travel_planes_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | origin_iata | string | ✅ | IATA code | e.g. "GVA" |
    | destination_iata | string | ✅ | IATA code | e.g. "JFK" |
    | user_institutional_id | string | ✅ | - | e.g. EPFL: SCIPER |
    | departure_date | string | ❌ | ISO format YYYY-MM-DD | e.g. "2025-05-15 if date format not recognized ignore row, id date not in the carbon report year ignore row. If None the row is considered. |
    | number_of_trips | int | ✅ | 1 ≤ int | e.g. 2 |
    | cabin_class | string | ✅ | within `first,business,economy` | e.g. business |
    | note | string | ❌ | - | contains the note if needed |

???+ info "travel_planes_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | category | string | ✅ | within `short_to_medium_haul, medium_to_long_haul` | e.g. "medium_to_long_haul" |
    | cabin_class | string | ✅ | business, economy, first | flight class to choose the corresponding factor. for EPFL: Premium economy is treated as economy, first class and business class have the same factor for short to mediul haul |
    | ef_kg_co2eq_per_km | float | ✅ | 0 ≤ float | e.g. "0.345" |
    | rfi_adjustment | float | ✅ | 0 ≤ float | The RFI (Radiative Forcing Index) for the methodology should be specified here, e.g. 2, 2.7, 3, etc. This is used to account for the total warming impact of flying. for EPFL: 1.35, which corresponds to transforming mobitool factors to 2.7 RFI as in atmosfair |
    | min_distance | float | ✅ | in km, unique value | e.g. "300" min distance of the category |
    | max_distance | float | ✅ | in km, unique value | e.g. "1200" max distance of the category |

???+ info "travel_planes_locations_reference.csv"

    !!! info "Data source"
        Airport location reference data is sourced from [GeoNames](https://www.geonames.org/), a geographical database covering all countries.

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | name | string | ✅ | - | name of the location, typically the city or train station |
    | airport_size | string | ❌ | within `medium_airport,large_airport` | e.g. "medium_airport" |
    |latitude | float | ✅ | - | e.g. 46.2044 |
    | longitude | float | ✅ | - | e.g. 6.1432 |
    | continent | string | ❌ | within `EU,NA,SA,AF,AS,OC` | e.g. "EU" |
    | country_code | string | ❌ | in `ISO 3166-1 alpha-2` format or use `RoW` for rest of the world | e.g. "CH" |
    | iata_code | string | ✅ | IATA code of the airport | e.g. "GVA" |
    | municipality | string | ❌ | - | e.g. "Geneva" |
    | keywords | string | ❌ | - | keywords to link the location with the possible different names in the data |

???+ info "travel_trains_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits,numbers only) |
    | origin_name | string | ✅ | train station name | e.g. "Geneve Cornavin" TBD  it accepts error in naming |
    | origin_country_code | string | ✅ | ISO2 for the country  | e.g. CH for switzerland|
    | destination_name | string | ✅ | train station name | e.g. "Geneve Cornavin" TBD  it accepts error in naming |
    | destination_country_code | string | ✅ |ISO2 for the country | e.g. CH for switzerland |
    | user_institutional_id | string | ✅ | only number | e.g. EPFL: SCIPER, if not in Headcount ignore row with a warning message 'SCIPER for this trip not in unit. Add the SCIPER in the Headcount module.' |
    | departure_date | string | ❌ | ISO format YYYY-MM-DD | e.g. "2025-05-15 if date format not recognized ignore row, id date not in the carbon report year ignore row. If None the row is considered. |
    | number_of_trips | int | ✅ | 1 ≤ int | e.g. 2 |
    | cabin_class | string | ✅ | within `first,second` | e.g. second |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |

    !!! note
        Traveler name is obtained with headcount

???+ info "travel_trains_test.csv , travel_trains_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | origin_name | string | ✅ | train station name | e.g. "Geneve Cornavin" TBD  it accepts error in naming |
    | origin_country_code | string | ✅ | ISO2 for the country  | e.g. CH for switzerland|
    | destination_name | string | ✅ | train station name | e.g. "Geneve Cornavin" TBD  it accepts error in naming |
    | destination_country_code | string | ✅ |ISO2 for the country | e.g. CH for switzerland |
    | user_institutional_id | string | ✅ | only number | e.g. EPFL: SCIPER, if not in Headcount row ignore |
    | departure_date | string | ❌ | ISO format YYYY-MM-DD | e.g. "2025-05-15 if date format not recognized ignore row, id date not in the carbon report year ignore row. If None the row is considered. |
    | number_of_trips | int | ✅ | 1 ≤ int | e.g. 2 |
    | cabin_class | string | ✅ | within `first,second` | e.g. second |
    | note | string | ❌ | - | contains the note if needed |

???+ info "travel_trains_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | country_code | string | ✅ | in `ISO 3166-1 alpha-2` format or use `RoW` for rest of the world | e.g. "CH" |
    | ef_kg_co2eq_per_km | float | ✅ | 0 ≤ float | e.g. "0.125" |

???+ info "travel_trains_locations_reference.csv"

    !!! info "Data source"
        Train station location reference data is sourced from [GeoNames](https://www.geonames.org/), a geographical database covering all countries.

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | name | string | ✅ | - | name of the location, typically the city or train station |
    |latitude | float | ✅ | - | e.g. 46.2044 |
    | longitude | float | ✅ | - | e.g. 6.1432 |
    | continent | string | ❌ | within `EU,NA,SA,AF,AS,OC` | e.g. "EU" |
    | country_code | string | ✅ | in `ISO 3166-1 alpha-2` format or use `RoW` for rest of the world | e.g. "CH" |
    | municipality | string | ❌ | - | e.g. "Geneva" |
    | keywords | string | ❌ | - | keywords to link the location with the possible different names in the data |

---


## Research Facilities

???+ info "researchfacilities_common_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits, numbers only) |
    | researchfacility_id | string | ✅ | within `researchfacilities_common_factors.csv` | e.g. for EPFL: cf of research facilities |
    | researchfacility_name | string | ✅ | within `researchfacilities_common_factors.csv` | e.g. CIBM-GE |
    | use | float | ✅ | 0 ≤ float, in format of `use_unit` | e.g. 34 |
    | use_unit | string | ✅ | within `researchfacilities_common_factors.csv` | e.g. chf, hours... if correspondence is not found in factors, the row is ignored with a warning message |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line. in this case this would correspond to the co2 generated by the unit use of the facility |

???+ info "researchfacilities_common_test.csv and researchfacilities_common_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | researchfacility_id | string | ✅ | within `researchfacilities_common_factors.csv` | e.g. for EPFL: cf of research facilities |
    | researchfacility_name | string | ✅ | within `researchfacilities_common_factors.csv` | e.g. CIBM-GE |
    | use | float | ✅ | 0 ≤ float, in format of `use_unit` | e.g. 34 |
    | use_unit | string | ✅ | within `researchfacilities_common_factors.csv` | e.g. chf, hours... if correspondence is not found in factors, the row is ignored with a warning message |
    | note | string | ❌ | - | contains the note if needed |

???+ info "researchfacilities_common_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | researchfacility_id | string | ✅ | | e.g. for EPFL: cf of research facilities |
    | researchfacility_name | string | ✅ | | e.g. CIBM-GE |
    | kg_co2eq_sum | float | ❌ | 0 ≤ float | e.g. 3555, not mandatory because the calculator is used to compute these results. however if this quantity is given here, it is taken instead of the calculator computation. It corresponds to the co2 for the research facility coming from purchases, equipment, buildings, process emissions and combustion emissions. |
    | total_use | float | ✅ | 0 ≤ float, in the unit of "use_unit" | e.g. 34 |
    | use_unit | string | ✅ | - | typically currency, time or count : `chf` if total_use calculated by billing, or time in `hrs` or other. This info appears in the table for the users. |

???+ info "researchfacilities_animals_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits, numbers only) |
    | researchfacility_id | string | ✅ | within `researchfacilities_animals_factors.csv` | e.g. for EPFL: cf of research facilities |
    | researchfacility_name | string | ✅ | within `researchfacilities_animals_factors.csv` | e.g. CIBM-GE |
    | researchfacility_type | string | ✅ | within `researchfacilities_animals_factors.csv` | e.g. mice |
    | use | float | ✅ | 0 ≤ float, in format of `use_unit` | e.g. 34 |
    | use_unit | string | ✅ | within `researchfacilities_animals_factors.csv` | e.g. housing |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |

???+ info "researchfacilities_animals_test.csv and researchfacilities_animals_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | researchfacility_id | string | ✅ | within `researchfacilities_animals_factors.csv` | e.g. for EPFL: cf of research facilities |
    | researchfacility_name | string | ✅ | within `researchfacilities_animals_factors.csv` | e.g. CIBM-GE |
    | researchfacility_type | string | ✅ | within `researchfacilities_animals_factors.csv` | e.g. mice |
    | use | float | ✅ | 0 ≤ float, in format of `use_unit` | e.g. 34 |
    | use_unit | string | ✅ | within `researchfacilities_animals_factors.csv` | e.g. housings |
    | note | string | ❌ | - | contains the note if needed |

???+ info "researchfacilities_animals_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | researchfacility_id | string | ✅ | | e.g. for EPFL: cf of research facilities |
    | researchfacility_name | string | ✅ | | e.g. CIBM-GE |
    | processemissions_share | float | ✅ | 0 ≤ float ≤ 1 | e.g. 0.3, this is the share of process emissions in the total co2eq of the research facility. This is used to split the total co2eq into the different parts of the research facility (for animal facility, mice vs fish). |
    | building_energycombustions_share | float | ✅ | 0 ≤ float ≤ 1 | e.g. 0.3, this is the share of building energy combustion emissions in the total co2eq of the research facility. This is used to split the total co2eq into the different parts of the research facility (for animal facility, mice vs fish).|
    | building_rooms_share | float | ✅ | 0 ≤ float ≤ 1 | e.g. 0.1, this is the share of building rooms emissions in the total co2eq of the research facility. This is used to split the total co2eq into the different parts of the research facility (for animal facility, mice vs fish). |
    | purchases_common_share | float | ✅ | 0 ≤ float ≤ 1 | e.g. 0.2, this is the share of purchases common emissions in the total co2eq of the research facility. This is used to split the total co2eq into the different parts of the research facility (for animal facility, mice vs fish). |
    | purchases_additional_share | float | ✅ | 0 ≤ float ≤ 1 | e.g. 0.05, this is the share of purchases additional emissions in the total co2eq of the research facility. This is used to split the total co2eq into the different parts of the research facility (for animal facility, mice vs fish). |
    | equipments_share | float | ✅ | 0 ≤ float ≤ 1 | e.g. 0.15, this is the share of equipments emissions in the total co2eq of the research facility. This is used to split the total co2eq into the different parts of the research facility (for animal facility, mice vs fish). |
    | kg_co2eq_sum_processemissions | float | ❌ | 0 ≤ float | e.g. 3555, not mandatory because the calculator is used to compute these results. however if this quantity is given here, it is taken instead of the calculator computation. It corresponds to the co2 for the research facility coming from process emissions |
    | kg_co2eq_sum_building_energycombustions | float | ❌ | 0 ≤ float | e.g. 3555, not mandatory because the calculator is used to compute these results. however if this quantity is given here, it is taken instead of the calculator computation. It corresponds to the co2 for the research facility coming from buidlings energy combustion |
    | kg_co2eq_sum_building_rooms | float | ❌ | 0 ≤ float | e.g. 3555, not mandatory because the calculator is used to compute these results. however if this quantity is given here, it is taken instead of the calculator computation. It corresponds to the co2 for the research facility coming from building rooms |
    | kg_co2eq_sum_purchases_common | float | ❌ | 0 ≤ float | e.g. 3555, not mandatory because the calculator is used to compute these results. however if this quantity is given here, it is taken instead of the calculator computation. It corresponds to the co2 for the research facility coming from purchases common |
    | kg_co2eq_sum_purchases_additional | float | ❌ | 0 ≤ float | e.g. 3555, not mandatory because the calculator is used to compute these results. however if this quantity is given here, it is taken instead of the calculator computation. It corresponds to the co2 for the research facility coming from purchases additional |
    | kg_co2eq_sum_equipments | float | ❌ | 0 ≤ float | e.g. 3555, not mandatory because the calculator is used to compute these results. however if this quantity is given here, it is taken instead of the calculator computation. It corresponds to the co2 for the research facility coming from equipments |
    | researchfacility_type | string | ✅ | - | e.g. mice |
    | total_use | float | ✅ | 0 ≤ float, in the unit of "use_unit" | e.g. 34 |
    | use_unit | string | ✅ | - | e.g. housing |

---

## Additional Categories

### Food, commuting and waste

These categories are related to the headcount. The total FTE is used to compute the values. For the logic of the calculation, all three categories - food, commuting, waste - are in the same files. A difference is made between student and members of staff, but waste has no difference in values.

???+ info "headcount_members_factors.csv" & "headcount_students_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | headcount_category | string | ✅ | within `food, commuting, waste` | e.g. food |
    | headcount_class | string | ✅ | within the class of the categories, food: `vegetarian, non_vegetarian` , commuting: `walking, cycling, powered_two_wheeler, public_transport, car`, waste: `incineration, composting, biogas, recycling` | the class of the category|
    | headcount_subclass | string | ❌ | none | the subclass of the category if needed |
    | number_of_unit_per_fte | float | ✅ | 0 ≤ float | e.g. for food, this is the kg of food per FTE, for commuting this is the km per FTE, for waste this is the kg of waste per FTE |
    | ef_kg_co2eq_per_unit | float | ✅ | 0 ≤ float | e.g. for food, this is the kg of co2eq per kg of food, for commuting this is the kg of co2eq per km, for waste this is the kg of co2eq per kg of waste |
    | unit | string | ✅ | - | e.g. for food, this is kg, for commuting this is km, for waste this is kg |

    !!! note
        No test and template files

### Building grey energy

???+ info "building_construction_renovation_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | building_name | string | ✅ | within `building_rooms_reference.csv` | e.g. BCH |
    | category | string | ✅ | within the class of the categories `new-tech, new-env,ren-tech,ren-env,demolition` | the category of the specified category of grey energy |
    | ef_kgco2eq_per_m2 | float | ✅ | 0 ≤ float | the kg of co2eq per m2 attributes to a specified category of grey energy |

---
