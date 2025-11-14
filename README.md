# SQL-
## Webtraffic_analysis

CREATE SCHEMA Website_traffic_analysis;

USE Website_traffic_analysis;

CREATE TABLE User_sessions (
    Session_date DATE,
    User_Id VARCHAR(50),
    Traffic_source VARCHAR(100),
    Location_country VARCHAR(50),
    Session_duration FLOAT,
    Is_bounced BOOLEAN,
    Is_converted BOOLEAN
);


UPDATE user_sessions
SET traffic_source = 'Direct / None'
WHERE traffic_source IS NULL;

INSERT INTO user_sessions 
(Session_date, User_id, Traffic_source, Location_country, Session_duration, Is_bounced, Is_converted) 
VALUES
('2025-11-13', 'A100', 'Google / organic', 'India', 45, 0, 0),
('2025-11-13', 'B200', 'Facebook / cpc', 'USA', 150, 0, 1),
('2025-11-13', 'C300', 'Google / organic', 'India', 5, 1, 0),
('2025-11-13', 'D400', 'Direct / None', 'UK', 90, 0, 1),
('2025-11-13', 'E500', 'Facebook / cpc', 'USA', 20, 1, 0),
('2025-11-13', 'F600', 'Direct / None', 'UK', 120, 0, 0),
('2025-11-13', 'G700', 'Google / organic', 'India', 180, 0, 1),
('2025-11-13', 'H800', 'Facebook / cpc', 'Canada', 10, 1, 0);

TRUNCATE TABLE User_sessions;

SELECT traffic_source, COUNT(*) AS Total_sessions
FROM user_sessions
GROUP BY traffic_source;

SELECT Traffic_source, SUM(Is_Converted) AS Total_conversion
FROM User_sessions
GROUP BY Traffic_source;

SELECT Traffic_source,
    (SUM(Is_Converted) / COUNT(*)) * 100 AS Total_conversion_rate
FROM User_sessions
GROUP BY Traffic_source;

SELECT Traffic_source,
    ROUND((SUM(Is_Converted) / COUNT(*)) * 100, 2) AS Total_conversion_rate
FROM User_sessions
GROUP BY Traffic_source;

SELECT Traffic_source,
    COUNT(*) AS Total_sessions,
    SUM(Is_Converted) AS Total_Conversions,
    ROUND((AVG(Is_converted)) * 100, 2) AS Total_Conversion_rate,
    ROUND((AVG(Is_bounced)) * 100, 2) AS Total_Bounced_rate
FROM user_sessions
GROUP BY Traffic_source;


CREATE VIEW traffic_kpi_summary AS
(
    SELECT Traffic_source,
        COUNT(*) AS Total_sessions,
        SUM(Is_Converted) AS Total_Conversions,
        ROUND((AVG(Is_converted)) * 100, 2) AS Total_Conversion_rate,
        ROUND((AVG(Is_bounced)) * 100, 2) AS Total_Bounced_rate
    FROM user_sessions
    GROUP BY Traffic_source
);
