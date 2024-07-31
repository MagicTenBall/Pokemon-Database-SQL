# Pokemon-Database-SQL
# Brainstation Data Analytics project

SELECT DISTINCT COUNT(Pokemon_Name) AS 'Unique Pokemon'
FROM final_pokemon_data_for_sql_may23;
-- 1379
ALTER TABLE Columns
RENAME COLUMN Secondary_Ability_Description TO Secondary_Ability_Description
FROM final_pokemon_data_for_sql_may23;

-- Edit through the right click of db (alter table;
then edit column name)
UPDATE final_pokemon_data_for_sql_may23
SET
Pokemon_Name = REPLACE(Pokemon_Name, '"', ''), Legendary_Type = REPLACE(Legendary_Type, '"', ''), Primary_Type = REPLACE(Primary_Type, '"', ''), Secondary_Type = REPLACE(Secondary_Type, '"', ''), Primary_Ability = REPLACE(Primary_Ability, '"', ''),
Primary_Ability_Description = REPLACE(Primary_Ability_Description, '"', ''), Secondary_Ability = REPLACE(Secondary_Ability, '"', ''),
Secondary_Ability_Description = REPLACE(Secondary_Ability_Description, '"', ''), Hidden_Ability = REPLACE(Hidden_Ability, '"', ''), Hidden_Ability_Description = REPLACE(Hidden_Ability_Description, '"', ''),
Special_Event_Ability = REPLACE(Special_Event_Ability, '"', ''), Special_Event_Ability_Description = REPLACE(Special_Event_Ability_Description, '"', ''),
Primary_Egg_Group = REPLACE(Primary_Egg_Group, '"', ''),
Secondary_Egg_Group = REPLACE(Secondary_Egg_Group, '"', '');

-- Use Cases for High, Medium, Low (Option 1)
SELECT Attack_Stat
IF (Attack_Stat > 100) then 'High' elseif (Attack_Stat < 80) then 'Low' AS Attack_Stat_Level FROM final_pokemon_data_for_sql_may23;

-- Use Cases for High, Medium, Low (Option 3) SELECT Health_Stat, Attack_Stat, Defense_Stat, Special_Attack Stat, Special_Defense_Stat, Speed_Stat,
CASE
WHEN (Health_Stat < 90) THEN 'Low'
WHEN (Health_Stat >= 90) AND (Health_Stat < 110) THEN 'Medium'
WHEN (Health_Stat >= 110) THEN 'High'
WHEN (Attack_Stat < 90) THEN 'Low'
WHEN (Attack_Stat >= 90) AND (Attack_Stat < 110)

THEN 'Medium'
WHEN (Attack_Stat >= 110) THEN 'High'
END AS Attack_Stat_Level
WHEN (Defense_Stat < 90) THEN 'Low'
WHEN (Defense_Stat >= 90) AND (Defense_Stat < 110) THEN 'Medium'
WHEN (Defense_Stat >= 110) THEN 'High'
END AS Defense_Stat_Level
WHEN (Special_Attack_Stat < 90) THEN 'Low'
WHEN (Special_Attack_Stat >= 90) AND (Special_Attack_Stat < 110) THEN 'Medium'
WHEN (Special_Attack_Stat >= 110) THEN 'High'
END AS Special_Attack_Stat_Level
WHEN (Special_Defense_Stat < 90) THEN 'Low'
WHEN (Special_Defense_Stat >= 90) AND (Special_Defense_Stat < 110) THEN 'Medium'
WHEN (Special_Defense_Stat >= 110) THEN 'High'
END AS Special_Defense_Stat_Level
WHEN (Speed_Stat < 90) THEN 'Low'
WHEN (Speed_Stat >= 90) AND (Speed_Stat < 110) THEN 'Medium'
WHEN (Speed_Stat >= 110) THEN 'High'
END AS Health_Stat_Level,
END AS Speed_Stat_Level
FROM final_pokemon_data_for_sql_may23;
-- Use Cases for High, Medium, Low (Option 2) SELECT Health_Stat,
CASE

WHEN (Health_Stat < 90) THEN 'Low'
WHEN (Health_Stat >= 90) AND (Health_Stat < 110) THEN 'Medium'
WHEN (Health_Stat >= 110) THEN 'High'
END AS Health_Stat_Level
FROM final_pokemon_data_for_sql_may23;
SELECT Attack_Stat,
CASE
WHEN (Attack_Stat < 90) THEN 'Low'
WHEN (Attack_Stat >= 90) AND (Attack_Stat < 110) THEN 'Medium'
WHEN (Attack_Stat >= 110) THEN 'High'
END AS Attack_Stat_Level
FROM final_pokemon_data_for_sql_may23;
SELECT Defense_Stat,
CASE
WHEN (Defense_Stat < 90) THEN 'Low'
WHEN (Defense_Stat >= 90) AND (Defense_Stat < 110) THEN 'Medium'
WHEN (Defense_Stat >= 110) THEN 'High'
END AS Defense_Stat_Level
FROM final_pokemon_data_for_sql_may23;
SELECT Special_Attack_Stat,
CASE
WHEN (Special_Attack_Stat < 90) THEN 'Low' WHEN (Special_Attack_Stat >= 90) AND (Special_Attack_Stat < 110) THEN 'Medium'

WHEN (Special_Attack_Stat >= 110) THEN 'High' END AS Special_Attack_Stat_Level
FROM final_pokemon_data_for_sql_may23;
SELECT Special_Defense_Stat,
CASE
WHEN (Special_Defense_Stat < 90) THEN 'Low' WHEN (Special_Defense_Stat >= 90) AND (Special_Defense_Stat < 110) THEN 'Medium' WHEN (Special_Defense_Stat >= 110) THEN 'High' END AS Special_Defense_Stat_Level
FROM final_pokemon_data_for_sql_may23;
SELECT Speed_Stat,
CASE
WHEN (Speed_Stat < 90) THEN 'Low'
WHEN (Speed_Stat >= 90) AND (Speed_Stat < 110) THEN 'Medium'
WHEN (Speed_Stat >= 110) THEN 'High'
END AS Speed_Stat_Level
FROM final_pokemon_data_for_sql_may23;
-- Supply the count for each high, medium, low
//Finding duplicates
Select Pokedex_Number, Pokemon_Name,Primary_Type, Secondary_Type, Health_Stat, Attack_Stat, Defense_Stat,
                Special_Attack_Stat,

Special_Defense_Stat, Speed_Stat, Base_Stat_Total, Count(*)
From pokemon_database_fp
Group By Pokedex_Number, Pokemon_Name,Primary_Type, Secondary_Type, Health_Stat, Attack_Stat, Defense_Stat,
Special_Attack_Stat, Special_Defense_Stat, Speed_Stat, Base_Stat_Total
Having count(*) > 1;
//create stored procedure for pokemon typing
Delimiter $$
create procedure  type_reader(in typing text)
BEGIN
        Select *
    From pokemon_database_fp p
    where p.Primary_Type = typing or
p.Secondary_Type = typing;
END$$
Delimiter ;
//Delete duplicates
Delete p1
from pokemon_database_fp p1, pokemon_database_fp p2 where (p1.Pokedex_Number = p2.Pokedex_Number
and p1.Attack_Stat = p2.Attack_Stat

and p1.Base_Stat_Total = p2.Base_Stat_Total
and p1.Defense_Stat = p2.Defense_Stat
and p1.Health_Stat = p2.Health_Stat
and p1.Special_Attack_Stat = p2.Special_Attack_Stat and p1.Special_Defense_Stat = p2.Special_Defense_Stat
and p1.Speed_Stat = p2.Speed_Stat
and p1.Pokemon_Id > p2.Pokemon_Id);

-- Top 10 for Health, Attack, Defense, Special
Attack, Special Defense, Speed

-- Then Total Top 10
