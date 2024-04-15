# Яндекс.Практикум (Спринт 5. Основы баз данных)

Задача Спринта 5: проанализировать данные о фондах и инвестициях и создание запросов к базе.


Задание: 1/8:

Посчитай, сколько компаний закрылось.

Ответ: SELECT COUNT(STATUS)
       FROM company
       WHERE STATUS LIKE '%closed%'

Задание: 2/8:

Отобрази количество привлечённых средств для новостных компаний США. Используй данные из таблицы company. Отсортируй таблицу по убыванию значений в поле funding_total.

Ответ: SELECT funding_total
       FROM company
           WHERE category_code LIKE 'news'
           AND country_code LIKE 'USA'
       ORDER BY funding_total DESC

Задание: 3/8:

Отобрази имя, фамилию и названия аккаунтов людей в поле network_username, которые начинаются на 'Silver'.

Ответ: SELECT first_name,
              last_name,
              network_username
       FROM people
       WHERE network_username LIKE 'Silver%'

Задание: 4/8:

Выведи на экран всю информацию о людях, у которых названия аккаунтов в поле network_username содержат подстроку 'money', а фамилия начинается на 'K'.

Ответ: SELECT *
       FROM people
            WHERE network_username LIKE '%money%'
            AND last_name LIKE 'K%'

Задание: 5/8:

Для каждой страны отобрази общую сумму привлечённых инвестиций, которые получили компании, зарегистрированные в этой стране. Страну, в которой зарегистрирована компания, можно определить по коду страны. Отсортируй данные по убыванию суммы.

Ответ: SELECT country_code,
          SUM(funding_total)
       FROM company
       GROUP BY country_code
       ORDER BY SUM(funding_total) DESC

Задание: 6/8:

Отобрази имя и фамилию всех сотрудников стартапов. Добавь поле с названием учебного заведения, которое окончил сотрудник, если эта информация известна.

Ответ: SELECT pep.first_name,
              pep.last_name,
              ed.instituition
       FROM people AS pep
       LEFT JOIN education AS ed ON pep.id = ed.person_id

Задание: 7/8:

Найди общую сумму сделок по покупке одних компаний другими в долларах. Отбери сделки, которые осуществлялись только за наличные с 2011 по 2013 год включительно.

Ответ: SELECT SUM(price_amount)
       FROM acquisition
       WHERE term_code = 'cash' AND EXTRACT(year FROM acquired_at) IN (2011, 2012, 2013);

Задание: 8/8:

Выясни, в каких странах находятся фонды, которые чаще всего инвестируют в стартапы. 
Для каждой страны посчитай минимальное, максимальное и среднее число компаний, в которые инвестировали фонды этой страны, основанные с 2010 по 2012 год включительно. Исключи страны с фондами, у которых минимальное число компаний, получивших инвестиции, равно нулю. 
Выгрузи десять самых активных стран-инвесторов: отсортируй таблицу по среднему количеству компаний от большего к меньшему. Затем добавь сортировку по коду страны в лексикографическом порядке.

Ответ: SELECT country_code,
       MIN(invested_companies),
       MAX(invested_companies),
       AVG(invested_companies)
       FROM (SELECT *
            FROM fund
            WHERE EXTRACT (year FROM founded_at) BETWEEN 2010 AND 2012) AS f
       GROUP BY country_code
       HAVING MIN(invested_companies) > 0
       ORDER BY AVG(invested_companies) DESC
       LIMIT 10;
