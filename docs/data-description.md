# Data Management Guide
!!! warning "Under construction"
    This section is under development. So the content may be incomplete and subject to change. 

This guide provides comprehensive data validation schemas for all modules in the CO2 Calculator. These specifications help data managers prepare and upload data correctly.

---

## General Notes

!!! info "Data and factor files"
    - module_data.csv: data uploaded by the backmanagement office. Here it is important to always have the info of the unit_institutional_id. 
    - factors.csv: factors that are necessary for the calculation. (emission factors and/or other factors)
    - module_template.csv: file that has the names of the columns, and a line of example. Users can download it by clicking on "Download CSV Template".
    - module_test.csv: file that is uploaded via the button "Upload CSV". In this CSV, we have the same columns as in the CSV "template", (and there is no unit_institutional_id since the user is already inside the unit).

 

!!! info "Important Information"
    - **unit_institutional_id**: For institutional data where relevant, this field contains the unit identifier (e.g., for EPFL: cf_id as 4-digit numbers).
    - **kg_co2eq**: When this optional field is provided with a value, no calculation is performed for that line - the value is used directly.
    - **note**: Available across all modules to add any relevant comments or explanations for specific entries.
    - **Template and Test files**: Used for data entry by unit managers, while data files are typically pre-filled by the back-office.
    - **Factors files**: Contain emission factors and conversion coefficients required for calculations.

!!! warning "Data Validation"
    - Rows that don't meet mandatory field requirements or value constraints will be ignored during upload.
    - Warning messages will be displayed when data doesn't match factor files.
    - Date formats must follow ISO standard (YYYY-MM-DD).

---

## Headcount

???+ info "headcount_data.csv"

    | field | type | mandatory | values constraints | description |
    |-------|------|-----------|-------------------|-------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits numbers only) |
    | name | string | ✅ | non-empty string | e.g. First and second name |
    | position_title | string | ❌ | - | e.g. professor , if `None`, to be filled in the table by the user. This is the category that is shown in the table |
    | position_category | string | ❌ | within a list define ` professor, scientific_collaborator, postdoctoral_assistant, doctoral_assistant, trainee, technichal_administrative_staff, student, other` can be `None`. This is the category that is used to build the graph. | - |
    | user_institutional_id | string | ✅ | numbers only | for EPFL: sciper |
    | fte | float | ✅ | 0 ≤ float ≤ 1 | Full-time equivalent e.g. 1.0, 0.8 |
    | note | string | ❌ | - | contains the note if needed |

    !!! note
        position_category and fte can be completed in the table directly if not given and have to be completed to validate the module. 

???+ info "headcount_template.csv and headcount_test.csv"

    | field | type | mandatory | values constraints | description |
    |-------|------|-----------|-------------------|-------------|
    | name | string | ✅ | non-empty string | e.g. First and second name |
    | position_title | string | ❌ | - | e.g. professor , if `None`, to be filled in the table by the user. This is the category that is shown in the table |
    | position_category | string | ❌ | within a list define ` professor, scientific_collaborator, postdoctoral_assistant, doctoral_assistant, trainee, technichal_administrative_staff, student, other` can be `None`. This is the category that is used to build the graph. | - |
    | user_institutional_id | string | ✅ | numbers only | for EPFL: sciper |
    | fte | float | ✅ | 0 ≤ float ≤ 1 | Full-time equivalent e.g. 1.0, 0.8 |
    | note | string | ❌ | - | contains the note if needed |

---

## Equipment

???+ info "equipments_data.csv"

    | field | type | mandatory | values constraints | description |
    |-------|------|-----------|-------------------|-------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits numbers only) |
    | name | string | ✅ | non-empty string | e.g. name of the equipment "GoPro" |
    | equipment_class | string  | ✅ | within `equipments_factors.csv` | e.g. "Monitor" is used to get power in `equipments_factors.csv`. If `equipment_class` is not empty but the value is not listed to `equipments_factors.csv`, the row is ignored. A warning message is displayed in loading. |
    | sub_class | string  | ❌ | within `equipments_factors.csv`, can be `None` | e.g. "Monitor" , the sub_class is filled by the user. Must be in the tuples equipment_class/sub_class within `equipments_factors.csv` else row ignored - (Warning message in loading if data uploaded with the wrong subclass) |
    | active_usage_hours_per_week | int | ❌ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | standby_usage_hours_per_week | int | ❌ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given, no calculation is performed on this line |


???+ info "equipments_template.csv and equipments_test.csv"

    | field | type | mandatory | values constraints | description |
    |-------|------|-----------|-------------------|-------------|
    | name | string | ✅ | non-empty string | e.g. name of the equipment "GoPro" |
    | equipment_class | string  | ✅ | within `equipments_factors.csv` | e.g. "Monitor" is used to get power in `equipments_factors.csv`. If `equipment_class` is not empty but the value is not listed to `equipments_factors.csv`, the row is ignored. A warning message is displayed in loading. |
    | sub_class | string  | ❌ | within `equipments_factors.csv`, can be `None` |  e.g. "Monitor" , the sub_class is filled by the user. Must be in the tuples equipment_class/sub_class within `equipments_factors.csv` else row ignored - (Warning message in loading if data uploaded with the wrong subclass)|
    | active_usage_hours_per_week | int | ❌ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | standby_usage_hours_per_week | int | ❌ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | note | string | ❌ | - | contains the note if needed |

???+ info "equipments_factors.csv"

    | field | type | mandatory | values constraints | description |
    |-------|------|-----------|-------------------|-------------|
    | data_entry_type | string | ✅ | non-empty string within : {`scientific,it,other`} | e.g. scientific. Case-sensitive |
    | equipment_class | string  | ✅ | not empty string | e.g. "Evaporator" |
    | sub_class | string | ❌ | can be `None` | e.g. "ultra centrifuges" |
    | active_usage_hours_per_week | int | ✅ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | standby_usage_hours_per_week | int | ✅ | 0 ≤ int ≤ 168 | e.g. 23. The sum of active + passive must be ≤ 168 |
    | active_power_w | float | ✅ | ≥ 0 | e.g. 23 |
    | standby_power_w | float | ✅ | ≥ 0 | e.g. 23 |
    | ef_kg_co2eq_per_kwh | float | ✅ | ≥ 0 | e.g. swiss mix 0.125 |

---

## External Clouds & AI

???+ info "external_ai_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits numbers only) |
    | provider | string | ✅ | within `external_ai_factors.csv` | e.g. name of the firm "Google" |
    | usage_type | string | ✅ | within `external_ai_factors.csv` | e.g. "text,video,image", tuple provider/usage_type within `external_ai_factors`. If not in provided tuple, the raw is ignored with a Warning message. |
    | requests_per_user_per_day | string | ❌ | within "1-5 times per day", "5-20 times per day", "20-100 times per day", ">100 times per day" | e.g. 1-5 times per day |
    | user_count | float | ✅ | 1 ≤ float | e.g. 2. By default the numbers shown is the total headcount of the unit. |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed |

???+ info "external_ai_test.csv and external_ai_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | provider | string | ✅ | have to be in `external_ai_factors.csv` | e.g. name of the firm "Google" |
    | usage_type | string | ✅ | have to be in `external_ai_factors.csv` | e.g. "text,video,image" |
    | requests_per_user_per_day | string | ❌ | within "1-5 times per day", "5-20 times per day", "20-100 times per day", ">100 times per day" | e.g. 1-5 times per day |
    | user_count | float | ✅ | 1 ≤ float | e.g. 2. By default the numbers shown is the total headcount of the unit. |
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
    | service_type | string | ✅ | within `external_clouds_factors.csv` | e.g. one of `storage,compute,virtualisation` |
    | provider | string | ✅ | within `external_clouds_factors.csv` | e.g."AWS" |
    | spent_amount | float | ✅ | 0 ≤ float | e.g. 299 |
    | currency | string | ✅ | in `chf, eur` format, not `None` | e.g. eur |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed |

???+ info "external_clouds_test.csv and external_clouds_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | service_type | string | ✅ | within `external_clouds_factors.csv` | e.g. one of `storage,compute,virtualisation` |
    | provider | string | ✅ | within `external_clouds_factors.csv` | e.g."AWS" |
    | spent_amount | float | ✅ | 0 ≤ float | e.g. 299 |
    | currency | string | ✅ | in `chf, eur` format, not `None` | e.g. eur |
    | note | string | ❌ | - | contains the note if need |

???+ info "external_clouds_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | service_type | string | ✅ | not empty | e.g. `storage,compute,virtualisation` |
    | provider | string | ✅ | not empty | e.g. firm |
    | currency | string | ✅ | in `chf, eur` format, not `None` | e.g. eur |
    | ef_kg_co2eq_per_currency | float | ✅ | 0 ≤ float | e.g. 0.8 |

---

## Travel

???+ info "travel_planes_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (numbers only) |
    | origin_iata | string | ✅ | IATA code | e.g. "GVA". |
    | destination_iata | string | ✅ | IATA code | e.g. "JFK".|
    | user_institutional_id | string | ✅ | only number | e.g. EPFL: SCIPER |
    | departure_date | string | ❌ | ISO format | e.g. "2025-05-15" if date format not recognized ignore row, id date not in the carbon report year ignore row. |
    | number_of_trips | int | ✅ | 1 ≤ int | e.g. 2 |
    | cabin_class | string | ✅ | within `first,business,eco` | e.g. business. For EPFL when taken from API, premium economy needs to be classifed as eco. |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line. |

???+ info "travel_planes_test.csv and travel_planes_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | origin_iata | string | ✅ | IATA code | e.g. "GVA" |
    | destination_iata | string | ✅ | IATA code | e.g. "JFK" |
    | user_institutional_id | string | ✅ | only number | e.g. EPFL: SCIPER |
    | departure_date | string | ❌ | ISO format | e.g. "2025-05-15" if date format not recognized ignore row, id date not in the carbon report year ignore row |
    | number_of_trips | int | ✅ | 1 ≤ int | e.g. 2 |
    | cabin_class | string | ✅ | within `first,business,eco` | e.g. business |
    | note | string | ❌ | - | contains the note if needed |

???+ info "travel_planes_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | category | string | ✅ | within `very_short_haul,short_haul,medium_haul,long_haul` | e.g. "very_short_haul" |
    | ef_kg_co2eq_per_km | float | ✅ | 0 ≤ float | e.g. "0.345" |
    | rfi_adjustement | float | ✅ | 0 ≤ float | The RFI (Radiative Forcing Index) for the methodology should be specified here, e.g. 2, 2.7, 3, etc. This is used to account for the total warming impact of flying. |
    | class_adjustement | float | ✅ | 0 ≤ float | adjustement factor to take into consideration the class of the flight.  |
    | min_distance | float | ✅ | in km, unique value | e.g. "300" min distance of the category |
    | max_distance | float | ✅ | in km, unique value | e.g. "1200" max distance of the category |

???+ info "travel_trains_locations_reference.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | country_code | string | ✅ | in `ISO 3166-1 alpha-2` format or use `RoW` for rest of the world | e.g. "CH" |
    | ef_kg_co2eq_per_km | float | ✅ | 0 ≤ float | e.g. "0.125" |

???+ info "travel_trains_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits,numbers only) |
    | origin_name | string | ✅ | train station name | e.g. "Geneve Cornavin" TBD  it accepts error in naming |
    | destination_name | string | ✅ | train station name | e.g. "Zurich" TBD it accepts error in naming |
    | user_institutional_id | string | ✅ | only number | e.g. EPFL: SCIPER, if not in Headcount ignore row with a warning message 'SCIPER for this trip not in unit. Add the SCIPER in the Headcount module.' |
    | departure_date | string | ❌ | ISO format | e.g. "2025-05-15" if date format not recognized ignore row, id date not in the carbon report year ignore row |
    | number_of_trips | int | ✅ | 1 ≤ int | e.g. 2 |
    | cabin_class | string | ✅ | within `first,second` | e.g. second |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |

    !!! note
        Traveler name is obtained with headcount

???+ info "travel_trains_test.csv , travel_trains_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | origin_name | string | ✅ | train station name | e.g. "Geneve Cornavin" TBD it accepts error in naming |
    | destination_name | string | ✅ | train station name | e.g. "Zurich"  TBD it accepts error in naming |
    | user_institutional_id | string | ✅ | only number | e.g. EPFL: SCIPER, if not in Headcount row ignore |
    | departure_date | string | ❌ | ISO format | e.g. "2025-05-15" if date format not recognized ignore row, id date not in the carbon report year ignore row |
    | number_of_trips | int | ✅ | 1 ≤ int | e.g. 2 |
    | cabin_class | string | ✅ | within `first,second` | e.g. second |
    | note | string | ❌ | - | contains the note if needed |

???+ info "travel_trains_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | country_code | string | ✅ | in `ISO 3166-1 alpha-2` format or use `RoW` for rest of the world | e.g. "CH" |
    | ef_kg_co2eq_per_km | float | ✅ | 0 ≤ float | e.g. "0.125" |

???+ info "travel_trains_locations_reference.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | country_code | string | ✅ | in `ISO 3166-1 alpha-2` format or use `RoW` for rest of the world | e.g. "CH" |
    | ef_kg_co2eq_per_km | float | ✅ | 0 ≤ float | e.g. "0.125" |
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
    | currency | string | ✅ | in `chf,eur,usd, gbp, aud, etc` format | e.g. eur |
    | purchase_institutional_code | string | ✅ | within `purchases_common_factors.csv` | e.g. UNSPSC code, as to within `purchases_common_factors.csv` |
    | purchase_institutional_description | string | ❌ | - | e.g. UNSPSC description, if not given compute with `purchases_common_factors.csv` |
    | purchase_additional_code | string | ❌ | within `purchases_common_factors.csv` | e.g. NACRES code, this column is used for the co2 emission calculations. For EPFL: data uploaded from the data manager need tohave this column. But users can add purchases without and a static mapping (contained in the factors) is used for UNSPCS -> NACRES. |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |

???+ info "purchases_common_test.csv and purchases_common_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | name | string | ✅ | non-empty string | e.g. "HP Novobook" |
    | supplier | string  | ❌ | - | e.g. "Bentley Systems International Ltd" |
    | quantity | float | ❌ | 0 ≤ float | e.g. 3 |
    | total_spent_amount | float | ✅ | 0 ≤ float | e.g. 3567 |
    | currency | string | ✅ | in `chf,eur,usd, gbp, aud, etc` format | e.g. eur |
    | purchase_institutional_code | string | ✅ | within `purchases_factors.csv` | e.g. UNSPSC code |
    | purchase_institutional_description | string | ❌ | - | e.g. UNSPSC description, if not given compute with `purchases_common_factors.csv` |
    | purchase_additional_code | string | ❌ | within `purchases_factors.csv` | e.g. NACRES code |
    | note | string | ❌ | - | contains the note if needed |

???+ info "purchases_common_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | currency | string | ✅ | in `chf,eur,usd, gbp, aud, etc` format | e.g. eur |
    | purchase_category | string | ✅ | within `it_equipment,other,scientific_equipment,services,vehicle,consumable_accessories,biological_chemical_gaseous_product` | e.g. `vehicle`. This columns is used to split the purchases into the subsections in the module. |
    | purchase_institutional_code | string | ✅ | - | e.g. UNSPSC code |
    | purchase_institutional_description | string |  |❌ decription of the code | e.g. |
    | purchase_additional_code | string | ✅ | - | e.g. NACRES code |
    | ef_kg_co2eq_per_currency | float | ✅ | 0 ≤ float | e.g. 0.1 |

???+ info "purchases_additional_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-dits, numbers only) |
    | name | string | ✅ | non-empty string | e.g. "Liquid nitrogen" |
    | unit | string | ✅ | - | e.g. liter |
    | coef_to_kg | float | ✅ | non negative | e.g. 3.05 |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |

???+ info "purchases_additional_test.csv and purchases_additional_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | name | string | ✅ | non-empty string | e.g. "Liquid nitrogen" |
    | unit | string | ✅ | - | e.g. liter |
    | coef_to_kg | float | ✅ | non negative | e.g. 3.05 |
    | note | string | ❌ | no constraints | contains the note if needed |

???+ info "purchases_additional_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | name | string | ✅ | non-empty string | e.g. "Liquid nitrogen" |
    | ef_kg_co2eq_per_kg | float | ✅ | 0 ≤ float | e.g. 0.1 |

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
    | kg_co2eq_sum | float | ❌ | 0 ≤ float | e.g. 3555, not mandatory because the calculator is used to compute these results. however if this quantity is given here, it is taken instead of the calculator computation. It corresponds to the co2 for the research facility coming from purchases, equipment, buildings, process emissions and combustion emissions. |
    | researchfacility_type | string | ✅ | - | e.g. mice |
    | total_use | float | ✅ | 0 ≤ float, in the unit of "use_unit" | e.g. 34 |
    | use_unit | string | ✅ | - | e.g. housing |

---

## Process Emissions

???+ info "processemissions_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits, numbers only) |
    | category | string | ✅ | within `processes_factors.csv` | e.g. Refrigerant |
    | subcategory | string | ✅ | within `processes_factors.csv`, can be `None`  | e.g. R145 |
    | quantity | float | ✅ | in kg, float >=0 | e.g. 34 |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |

???+ info "processemissions_test.csv and processemissions_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | category | string | ✅ | within `processes_factors.csv` | e.g. Refrigerant |
    | subcategory | string | ✅ | within `processes_factors.csv`, can be `None` | e.g. R145 |
    | quantity | float | ✅ | in kg, float >=0 | e.g. 34 |
    | note | string | ❌ | - | contains the note if needed |

???+ info "processemissions_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | category | string | ✅ | - | for EPFL: fluide friorigènes |
    | subcategory | string | ✅ | - | e.g. NF3 |
    | unit | string | ✅ | - | eg kg |
    | ef_kg_co2eq_per_unit | float | ✅ | 0 ≤ float | e.g. 23'500 (kg CO2 eq / kg for SF6) |

---

## Buildings

???+ info "building_rooms_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits, numbers only) |
    | building_location | string | ❌ | can be `None` | e.g. "ECUBLENS" |
    | building_name | string | ✅ | within `building_rooms_factors.csv` | e.g. GC |
    | room_name | string | ❌ | digit or name, can be `None` | if `None` ,can be filled in the table by the user but not mandatory |
    | room_type | string | ✅ | within `office, miscellaneous, laboratories, archives, libraries, auditoriums` or `None` | e.g. "office" if `None` must be filled in the table by the user |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line. For EPFL, this must be filled for SCITAS, RCP, etc |

???+ info "building_rooms_test.csv and building_rooms_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | building_location | string | ❌ | can be `None` | e.g. "ECUBLENS" |
    | building_name | string | ✅ | within `building_rooms_factors.csv` | e.g. GC |
    | room_name | string | ❌ | digit or name, can be None | if None completed in the table by the user |
    | room_type | string | ✅ | within `office, miscellaneous, laboratories, archives, libraries, auditoriums` or `None` | e.g. "office" if `None` completed in the table. If the correspondence building, room_name, room_type does not exist, use the kwh_per_square_meter for the building, room_type (the room_name does not influence the consumption). |
    | note | string | ❌ | - | contains the note if needed |

    
???+ info "building_rooms_reference.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | building_location | string | ❌ | can be `None` | e.g. "ECUBLENS" |
    | building_name | string | ✅ | within `building_rooms_factors.csv` | e.g. GC |
    | room_name | string | ✅ | digit or name, can be None | if None completed in the table by the user |
    | room_type | string | ✅ | within `office, miscellaneous, laboratories, archives, libraries, auditoriums` or `None` | e.g. "office" , can be change by the user, if changed the corresponding type in `building_rooms_factors.csv` is used (only type and building are taken to compute co2_eq). So it is the room type that is used by default. |
    | room_surface_square_meter | float | ✅ | 0 ≤ float | e.g. 12 |

    
???+ info "building_rooms_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | building_name | string | ✅ | - | for EPFL: BCH,BS... |
    | room_type | string | ✅ | within `office, miscellaneous, laboratories, archives, libraries, auditoriums` or `None` | e.g. "office"  |
    | heating_kwh_per_square_meter | float | ✅ | 0 ≤ float | e.g. 2.3. These are the consumption hypotheses in squared meters for the given building and type of room. This column gives the hypotheses for all buildings, which are used for the calculation also when the data are input by the users (via the upload .csv)  |
    | cooling_kwh_per_square_meter | float | ✅ | 0 ≤ float | e.g. 2.3. These are the consumption hypotheses in squared meters for the given building and type of room. This column gives the hypotheses for all buildings, which are used for the calculation also when the data are input by the users (via the upload .csv)  |
    | ventilation_kwh_per_square_meter | float | ✅ | 0 ≤ float | e.g. 2.3. These are the consumption hypotheses in squared meters for the given building and type of room. This column gives the hypotheses for all buildings, which are used for the calculation also when the data are input by the users (via the upload .csv)  |
    | lighting_kwh_per_square_meter | float | ✅ | 0 ≤ float | e.g. 2.3. These are the consumption hypotheses in squared meters for the given building and type of room. This column gives the hypotheses for all buildings, which are used for the calculation also when the data are input by the users (via the upload .csv)  |
    | ef_kg_co2eq_per_kwh | float | ✅ | 0 ≤ float | e.g. 0.125 |
    | energy_type | string | ✅ | `electric, thermal, etc` | e.g. "electric" |
    | conversion_factor | float | ✅ | can be `None` | e.g. 4 , if `None` consider as 1. This is an adjustement factor needed because in some cases the kwh are elec, in other thermique, etc. And we need to bring it to right type to use the emission factors properly.  |
    


???+ info "building_energycombustions_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit_institutional_id | string | ✅ | numbers only | for EPFL: cf_id (4-digits, numbers only) |
    | name | string | ✅ | within `building_combustions_factors.csv` | e.g. "natural gas" |
    | unit | string | ✅ | in SI format | eg kg , if couple "unit" "name" not found in `building_energycombustions_factors.csv` row ignore with warning message |
    | quantity | float | ✅ | 0 ≤ float | e.g. 34 |
    | note | string | ❌ | - | contains the note if needed |
    | kg_co2eq | float | ❌ | - | if given no calculation is performed for the line |


???+ info "building_energycombustions_test.csv & building_energycombustions_template.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | name | string | ✅ | within `building_combustions_factors.csv` | e.g. "gas naturel mix" |
    | unit | string | ✅ | in SI format | eg kg , if couple "unit" "name" not found in `building_energycombustions_factors.csv` row ignore with warning message |
    | quantity | float | ✅ | 0 ≤ float | e.g. 34 |
    | note | string | ❌ | - | contains the note if needed |

???+ info "building_energycombustions_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | unit | string | ✅ | in SI format | e.g. kWh |
    | name | string | ✅ | has to be unique | e.g. gas naturel mix |
    | ef_kg_co2eq_per_unit | float | ✅ | 0 ≤ float | e.g. 0.05 |

---

## Additional Categories

### Waste

???+ info "waste_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | disposal_method | string | ✅ | within `waste_factors.csv` | e.g. incineration, biogas |
    | waste_type | string | ✅ | within `waste_factors.csv` | e.g. glass, domestic... The tuple `disposal_method, waste_type` must be in `factors.csv`|
    | kg_per_fte | float | ✅ | 0 ≤ float | e.g. 344 |

    !!! note
        No test and template files for waste data

???+ info "waste_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | disposal_method | string | ✅ | - | e.g. incineration, biogas |
    | waste_type | string | ✅ | - | e.g. glass, domestic... |
    | ef_kg_co2eq_per_kg | float | ✅ | 0 ≤ float | e.g. 2333 |

### Commuting

???+ info "commuting_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | commuting_type | string | ✅ | within `commuting_factors.csv` | e.g. car, bike... |
    | person_type | string | ✅ | student or staff | e.g. student |
    | km_per_fte | float | ✅ | 0 ≤ float | e.g. 3222 |

    !!! note
        No test and template files for commuting data

???+ info "commuting_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | commuting_type | string | ✅ | - | e.g. bike, car, .. |
    | ef_kg_co2eq_per_km | float | ✅ | 0 ≤ float | e.g. 23 |

### Food

???+ info "food_data.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | food_type | string | ✅ | within `food_factors.csv` | e.g. vegetarian, non-vegetarian |
    | person_type | string | ✅ | student or staff | e.g. student |
    | kg_per_fte | float | ✅ | 0 ≤ float | e.g. 37 |

    !!! note
        No test and template files for food data

???+ info "food_factors.csv"

    | field | type | mandatory | values constraints | example / notes |
    |-------|------|-----------|-------------------|-------------------|
    | food_type | string | ✅ | - | e.g. vegetarian, non-vegetarian |
    | ef_kg_co2eq_per_kg | float | ✅ | 0 ≤ float | e.g. 45 |

---


