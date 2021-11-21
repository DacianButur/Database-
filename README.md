Prima data creez baza de date. --> create database if not exists FarmaceuticalWareHouse; use farmaceuticalWareHouse;

Creez tabelul Product si setem ca si primary key idProduct --> create table Product (
idProduct int primary key not null,
productName varchar(25),
idCategory int,
price int
);


Dupa accea creez tabelul Category --> create table Category (
idCategory int primary key auto_increment not null,
productCategory varchar(25)
);


Facem alter table la Product ca sa adaugam fk ca sa facem legatura intre tabele --> alter table product add foreign key(idCategory) references category(idCategory) on delete set null;

Creez tabelul client --> create table Client(
idClient int primary key auto_increment not null,
clientName varchar(25)
);

Creez tabelul orders --> create table Orders (
idCategory int,
idProduct int,
idClient int,
Date date,
productUnits int,
foreign key(idProduct) references product(idProduct) on delete set null,
foreign key(idCategory) references category(idCategory) on delete set null,
foreign key(idClient) references client(IdClient) on delete set null
);

Dupa care populam tabelele prin comenzile de insert --> 

 
 Dupa aceea vom face selecturile pentru a afla informatiile necesare -->
 -- select pentru cerinta nr.1
-- select ca sa aflam nr-ul de comenzi din luna august a farmaciei dona si valoare lor medie + valoarea absoluta
select orders.idClient,orders.date, orders.idProduct,product.price,orders.productUnits,client.clientName, orders.productUnits * product.price as totalPrice from orders join product on orders.idProduct = product.idProduct join client on orders.idClient = client.idClient where orders.idClient = 200 and month(date) = 08;

-- select ca sa aflam valoarea medie pe comanda ale celor 3 comenzi.
select orders.idClient,client.clientName, avg(orders.productUnits * product.price) as totalPrice from orders join product on orders.idProduct = product.idProduct join client on orders.idClient = client.idClient where orders.idClient = 200 and month(date) = 08;

-- select ca sa vedem suma totala a comenzilor
select orders.idClient, client.clientName, sum(orders.productUnits * product.price) as totalPrice from orders join product on orders.idProduct = product.idProduct join client on orders.idClient = client.idClient where orders.idClient = 200 and month(date) = 08;


-- select pentru cerinta nr. 2:
select orders.idClient, client.clientName, orders.idCategory,orders.date, orders.productUnits * product.price as totalPrice from orders join client on orders.idClient = client.idClient join product on orders.idProduct = product.idProduct where orders.idCategory = 2 and year(date) = 2021 and client.clientName = 'Farmacia_Vlad';
-- vedem ca farmacia Vlad a avut 3 comenzi de antibiotice in 2021 cu o medie de 8400RON pe comanda.

-- select pentru cerinta nr.3:
select orders.idClient, client.ClientName, sum(orders.productUnits * product.price) as totalPrice from orders join client on orders.idClient = client.idClient join product on orders.idProduct = product.IdProduct group by clientName order by totalPrice desc;
-- vedem ca farmacia care a cheltuit cei mai multi bani pe comenzi in 2021 a fost Farmacia Dona.
