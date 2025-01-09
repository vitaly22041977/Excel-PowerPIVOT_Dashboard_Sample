ЗАДАЧА: построить модель данных в Power Pivot,  рассчитать меры DAX, вывести данные в сводные таблицы																
																
1	Создать папку DATA_TRANS и сохранить в неё 3 датасета (…072023, …082023, …092023), выполнить подключение к папке через Power Query															
2	"Выполнить преобразование примера файла (исправить форматы, добавить расчётный столбец: sum_N = кол-во*цена
	ВАЖНО, какие форматы в запросе Power Q такие будут в Power P)"															
3	Загрузить в модель данных справочники: магазины, продукты, клиенты (через получение внешних данных)															
4	Создать таблицу дат – автоматический календарь, исправить период - указать только 2023 год															
5	Загрузить в модель данных таблицу tbl_kpi_plan, подключить к таблице дат и магазинов, настроить визуальное отображение KPI															
6	Создать меры - проверить вывод данных через сводные таблицы															

	создадим меру выполнения плана продаж по городам															
	%_plana = DIVIDE(Sum('DATA_TRANS'[sum_all]);SUM(KPI_plan[plan_sum])) - настроим ключевые показатели эффективности															
	создадим меру подсчёта уникальных карт клиентов женщин из Бреста															
	F_Brest = CALCULATE(DISTINCTCOUNT('DATA_TRANS'[card_N]);DATA_personal[City]="Brest";DATA_personal[gender]="F")															
	создадим меру подсчёта суммы скидки за предыдущий месяц															
	%_Disc_prev_M =divide(sum('DATA_TRANS'[disc_N]);CALCULATE(sum('DATA_TRANS'[disc_N]);PREVIOUSMONTH('Calendar'[Date]));1)-1															
	создадим меру отклонения активных клиентов за предыдущий месяц															
	%_MAU_prev_M =IFERROR(DISTINCTCOUNT('DATA_TRANS'[card_N])/CALCULATE(DISTINCTCOUNT('DATA_TRANS'[card_N]);PREVIOUSMONTH('Calendar'[Date]))-1;"-")															
	создадим меру - количество чеков на клиента															
	Frequency =DISTINCTCOUNT('DATA_TRANS'[trans_N])/DISTINCTCOUNT('DATA_TRANS'[card_N])															
	создадим меру % суммы скидки от всей суммы покупок															
	%_discount = sum('DATA_TRANS'[disc_N])/sum('DATA_TRANS'[sum_all])															
	создадим меру долей продаж по магазинам															
	доля_shop_sum = DIVIDE(sum('DATA_TRANS'[sum_all]);CALCULATE(sum('DATA_TRANS'[sum_all]);ALL('DATA_TRANS'[Shop_N])))															
