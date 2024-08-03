# Diplomado-U-d-C-Practica-Modulo-SQL

## Caso práctico SQL

-- El restaurante "Sabores del Mundo", ha completado 3 meses de ventas ininterrumpidas de sus platillos. 
Durante estos meses de ventas se han registrado las órdenes y detalle de ventas de sus platillos, lo que nos llama a hacer un análisis de sus ventas.

--Derivado del análisis de ventas se desprenden los siguientes cuestionamientos para el análisis.

### 1.- ¿CUALES SON LOS 10 PLATILLOS MÁS VENDIDOS DESDE LA EPERTURA? Derivado de dicho análisis, se encontró que la Hamburger, es el Platillo que más se vendido durante dicho periodo.

![image](https://github.com/user-attachments/assets/1ad7aa39-7c80-4dd4-811c-f182543f9eac)

SELECT
	DISTINCT Item_Name  ,  COUNT (Item_Name) Mas_Vendidos_por_Orden, 
	count(DISTINCT order_id) Total_de_Ordenes
FROM
	PUBLIC.ORDER_DETAILS AS OD
	LEFT JOIN PUBLIC.MENU_ITEMS AS MI ON OD.ITEM_ID = MI.MENU_ITEM_ID

where  MI.MENU_ITEM_ID is not null 
group by Item_Name 
order by COUNT (Item_Name) DESC LIMIT 10

### 2.- Por otra parte, Edamame, fue el platillo que se vendió en un número mayor de órdenes. Al ser este entre los artículos más vendidos, prever un buen pronóstico del surtido de ingredientes de este para evitar caer en desabasto.

![image](https://github.com/user-attachments/assets/3175694f-2d1b-424b-9f19-7118826dc6ed)

SELECT
	DISTINCT Item_Name  ,  COUNT (Item_Name) Mas_Vendidos_por_Orden , count(DISTINCT order_id) Total_order
FROM
	PUBLIC.ORDER_DETAILS AS OD
	LEFT JOIN PUBLIC.MENU_ITEMS AS MI ON OD.ITEM_ID = MI.MENU_ITEM_ID
where  MI.MENU_ITEM_ID is not null 
group by Item_Name 
order by count(DISTINCT order_id)  DESC 
LIMIT 10

### 3.- Según los registros de "Sabores del mundo", Chicken Tacos, es el platillo menos vendido.

![image](https://github.com/user-attachments/assets/2c39c8c4-ae93-4079-9f9b-ed0455d33be4)

### Nota: Impulsar a través de una buena compaña, la venta de este platillo.

SELECT
	DISTINCT Item_Name  ,  COUNT (Item_Name) Menos_Vendidos_por_Orden_id , count(DISTINCT order_id) Total_order
FROM
	PUBLIC.ORDER_DETAILS AS OD
	LEFT JOIN PUBLIC.MENU_ITEMS AS MI ON OD.ITEM_ID = MI.MENU_ITEM_ID
where  MI.MENU_ITEM_ID is not null 
group by Item_Name 
order by count(DISTINCT order_id)  ASC  
LIMIT 1

### 4.- De acuerdo con nuestro análisis, enero de 2023, es el mes con el mayor número de ordenes colocadas en el mercado, sin embargo, marzo de 2023 es el mes con el mayor número de ventas en importe.

Mes con el mayor número de ordenes vendidas.

![image](https://github.com/user-attachments/assets/a5f16eab-e664-4637-b668-ff464345cf15)

SELECT 
DISTINCT LEFT (TEXT(ORDER_DATE),7) Periodo, count(DISTINCT order_id) Ordenes, sum(Price) Importe
FROM
	PUBLIC.ORDER_DETAILS AS OD
	LEFT JOIN PUBLIC.MENU_ITEMS AS MI ON OD.ITEM_ID = MI.MENU_ITEM_ID
where  MI.MENU_ITEM_ID is not null 
group by DISTINCT LEFT (TEXT(ORDER_DATE),7)
order by count(DISTINCT order_id) desc 

Mes con el mayor importe de ventas.

![image](https://github.com/user-attachments/assets/027d6165-d58b-4e47-b246-20cb0c27a967)

SELECT 
DISTINCT LEFT (TEXT(ORDER_DATE),7) Periodo, count(DISTINCT order_id) Ordenes, sum(Price) Importe
FROM
	PUBLIC.ORDER_DETAILS AS OD
	LEFT JOIN PUBLIC.MENU_ITEMS AS MI ON OD.ITEM_ID = MI.MENU_ITEM_ID
where  MI.MENU_ITEM_ID is not null 
group by DISTINCT LEFT (TEXT(ORDER_DATE),7)
order by sum(Price) desc 

### 5.- Muestra el platillo más vendido en importe durante los 3 primeros meses de ventas y el platillo menos vendido también en los tres meses de operación.

![image](https://github.com/user-attachments/assets/d210d3d2-3f9b-496a-af09-84516fa0df55)

SELECT
	*
FROM
	(
		SELECT DISTINCT
			LEFT(TEXT(ORDER_DATE), 7) PERIODO,
			ITEM_NAME,
			SUM(PRICE) IMPORTE,
			'Mas_Vendidos' Top_Tres
		FROM
			PUBLIC.ORDER_DETAILS AS OD
			LEFT JOIN PUBLIC.MENU_ITEMS AS MI ON OD.ITEM_ID = MI.MENU_ITEM_ID
		WHERE
			MI.MENU_ITEM_ID IS NOT NULL
		GROUP BY DISTINCT
			LEFT(TEXT(ORDER_DATE), 7),
			ITEM_NAME
		ORDER BY
			SUM(PRICE) DESC
		LIMIT
			3
	) AS R1
UNION
SELECT
	*
FROM
	(
		SELECT DISTINCT
			LEFT(TEXT(ORDER_DATE), 7) PERIODO,
			ITEM_NAME,
			SUM(PRICE) IMPORTE,
			'Menos_Vendidos' Top_Tres
		FROM
			PUBLIC.ORDER_DETAILS AS OD
			LEFT JOIN PUBLIC.MENU_ITEMS AS MI ON OD.ITEM_ID = MI.MENU_ITEM_ID
		WHERE
			MI.MENU_ITEM_ID IS NOT NULL
		GROUP BY DISTINCT
			LEFT(TEXT(ORDER_DATE), 7),
			ITEM_NAME
		ORDER BY
			SUM(PRICE) ASC
		LIMIT
			3
	) R2
ORDER BY
	Top_Tres

