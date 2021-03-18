# Intermediate_SQL
Intermediate SQL Query

- Join using Having 

SELECT c.id, c.title, c.description, COUNT(t.id) trans_count, SUM(t.amount) amount_donation
FROM campaign c
LEFT JOIN transaction t
ON c.id = t.campaign_id
AND upper(t.status) = 'VERIFIED'
GROUP BY c.id, c.title, c.description
HAVING COUNT(t.id)>1


- Using sub query

SELECT *
FROM(
SELECT c.id, c.title, c.description, COUNT(t.id) trans_count, SUM(t.amount) amount_donation
FROM campaign c
LEFT JOIN transaction t
ON c.id = t.campaign_id
AND upper(t.status) = 'VERIFIED'
GROUP BY c.id, c.title, c.description
)donation
WHERE trans_count > (
  SELECT COUNT(t.id) trans_count
  FROM campaign c
  LEFT JOIN transaction t
  ON c.id = t.campaign_id
  AND upper(t.status) = 'VERIFIED'
  where c.id = 1
  GROUP BY c.id
)


- Using Union

SELECT c.id, c.title, c.description, COUNT(t.id) trans_count, SUM(t.amount) amount_donation, t.status
FROM campaign c
LEFT JOIN transaction t
ON c.id = t.campaign_id
AND upper(t.status) = 'VERIFIED'
GROUP BY c.id, c.title, c.description, t.status
UNION 
SELECT c.id, c.title, c.description, COUNT(t.id) trans_count, SUM(t.amount) amount_donation, t.status
FROM campaign c
LEFT JOIN transaction t
ON c.id = t.campaign_id
AND upper(t.status) = 'APPROVED'
GROUP BY c.id, c.title, c.description, t.status


- Sub query on Select statement

SELECT c.id, c.title, c.description, COUNT(t.id) verified_count, SUM(t.amount) amount_donation
, (SELECT COUNT(id) FROM transaction WHERE campaign_id = c.id AND STATUS='PENDING') pending_count
FROM campaign c
LEFT JOIN transaction t
ON c.id = t.campaign_id
AND upper(t.status) = 'VERIFIED'
GROUP BY c.id, c.title, c.description

