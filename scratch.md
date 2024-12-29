# PDL

This is the solution I tried to solve the LL-1

1. RERA PDL: returns the PDLs of the day up to the specified time and date in the requested and explicitly reported format

   <details>

   ```
   SELECT DISTINCT
        azienda.nome AS aziendaId,
		CONCAT(mr0740_personale.cognome,' ',mr0740_personale.nome) AS mr0740_personale_prepostoId, 
		mr0712_11.codice AS codice,
        (SELECT GROUP_CONCAT(usersAll.username SEPARATOR ' | ') FROM bkdrilling2.users AS usersAll INNER JOIN maerskcova.mr0712_11_historystatus AS pdlhistory ON usersAll.id = pdlhistory.usersId WHERE pdlhistory.mr0712_11_pdlstatusId = 2 AND maerskcova.mr0712_11_historystatus.mr0712_11Id = pdlhistory.mr0712_11Id) AS authorizingUsersId,
        (SELECT GROUP_CONCAT(DATE_FORMAT(maerskcova.mr0712_11_historystatus.momentDt, '%d/%m/%Y %H:%i:%s') SEPARATOR ' | ') FROM maerskcova.mr0712_11_historystatus WHERE mr0712_11Id = maerskcova.mr0712_11.id AND maerskcova.mr0712_11_historystatus.mr0712_11_pdlstatusId = 1) AS recordedMomentDt,
        (SELECT GROUP_CONCAT(DATE_FORMAT(maerskcova.mr0712_11_historystatus.momentDt, '%d/%m/%Y %H:%i:%s') SEPARATOR ' | ') FROM maerskcova.mr0712_11_historystatus WHERE mr0712_11Id = maerskcova.mr0712_11.id AND maerskcova.mr0712_11_historystatus.mr0712_11_pdlstatusId = 2) AS autorizedMomentDt,
        (SELECT GROUP_CONCAT(DATE_FORMAT(maerskcova.mr0712_11_historystatus.momentDt, '%d/%m/%Y %H:%i:%s') SEPARATOR ' | ') FROM maerskcova.mr0712_11_historystatus WHERE mr0712_11Id = maerskcova.mr0712_11.id AND maerskcova.mr0712_11_historystatus.mr0712_11_pdlstatusId = 3) AS closedMomentDt,
        (SELECT GROUP_CONCAT(DATE_FORMAT(maerskcova.mr0712_11_historystatus.momentDt, '%d/%m/%Y %H:%i:%s') SEPARATOR ' | ') FROM maerskcova.mr0712_11_historystatus WHERE mr0712_11Id = maerskcova.mr0712_11.id AND maerskcova.mr0712_11_historystatus.mr0712_11_pdlstatusId = 4) AS suspendedMomentDt,
        (SELECT GROUP_CONCAT(DATE_FORMAT(maerskcova.mr0712_11_historystatus.momentDt, '%d/%m/%Y %H:%i:%s') SEPARATOR ' | ') FROM maerskcova.mr0712_11_historystatus WHERE mr0712_11Id = maerskcova.mr0712_11.id AND maerskcova.mr0712_11_historystatus.mr0712_11_pdlstatusId = 5) AS suspendedToRenewMomentDt
		#mr0712_11_historystatus.momentDt AS momentDt,
		#mr0712_11_pdlstatus.label AS mr0712_11_pdlstatusId
	FROM maerskcova.mr0712_11 
	INNER JOIN maerskcova.azienda ON azienda.id=mr0712_11.aziendaId 
	INNER JOIN maerskcova.mr0712_11_requester ON mr0712_11_requester.id=mr0712_11.mr0712_11_requesterId 
	INNER JOIN maerskcova.mr0740_personale ON mr0740_personale.id=mr0712_11.mr0740_personale_prepostoId 
	INNER JOIN maerskcova.mr0712_11_forecastactivity ON mr0712_11_forecastactivity.mr0712_11Id=mr0712_11.id
    INNER JOIN maerskcova.mr0712_11_historystatus ON mr0712_11_historystatus.mr0712_11Id=mr0712_11.id
    INNER JOIN maerskcova.mr0712_11_pdlstatus ON mr0712_11_pdlstatus.id=mr0712_11_historystatus.mr0712_11_pdlstatusId
	WHERE
		mr0712_11_forecastactivity.date = '2024-12-19' AND (mr0712_11_historystatus.momentDt BETWEEN '2024-12-19 00:00:01' AND '2024-12-19 23:59:59') AND mr0712_11_macroareeId=5 AND mr0712_11_compartmentId = 1
	ORDER BY maerskcova.mr0712_11_historystatus.momentDt
   
   ```
   </details>
