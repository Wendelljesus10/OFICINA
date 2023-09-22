# OFICINA
Construa um Projeto Lógico de Banco de Dados do Zero

-- CONSTRUÇÃO DO BANCO DE DADOS E SUAS TABELAS
create database oficer;

use oficer;

CREATE TABLE Clients(
  Client_id INT auto_increment NOT NULL,
  CPF CHAR(11) NOT NULL,
  Fname VARCHAR(45) NOT NULL,
  Lname VARCHAR(45) NOT NULL,
  Address VARCHAR(45) NULL,
  Numbers CHAR(11) NOT NULL,
  PRIMARY KEY (Client_id),
  constraint unique_CPF unique (CPF)
  );
  alter table Clients auto_increment = 1;
  
  CREATE TABLE Vehicle (
  Vehicle_id INT auto_increment NOT NULL,
  Nameveh VARCHAR(45) NOT NULL,
  Placa VARCHAR(7) NOT NULL,
  Typecar enum('Carro','Moto','Caminhão') NOT NULL,
  VClient_ID INT NOT NULL,
  PRIMARY KEY (Vehicle_id, VClient_ID),
  constraint Placa_UNIQUE unique (Placa),
  constraint fk_client_vehicle foreign key ( VClient_ID) references Clients (Client_id)
  );

  alter table Vehicle auto_increment = 1;
  
  CREATE TABLE Service (
  Service_id INT auto_increment NOT NULL,
  SDescription VARCHAR(45) NOT NULL,
  PRIMARY KEY (Service_id)
  );
alter table Service auto_increment = 1;

CREATE TABLE Parts_vehicle (
  Parts_id INT auto_increment NOT NULL,
  Name VARCHAR(45) NOT NULL,
  PRIMARY KEY (Parts_id)
  );

alter table Parts_vehicle auto_increment = 1;

CREATE TABLE Mechanic (
  Mechanic_id INT auto_increment NOT NULL,
  MName VARCHAR(45) NULL,
  Speciality enum('Manutenção','Montador','Auxiliar de Produção','Ajudante de Mecânico') NOT NULL,
  PRIMARY KEY (Mechanic_id),
  constraint Speciality_UNIQUE unique (Speciality)
  );
alter table Mechanic auto_increment = 1;

CREATE TABLE TEAM (
  TMechanic_id INT NOT NULL,
  TService_id INT NOT NULL,
  PRIMARY KEY (TService_id, TMechanic_id),
  constraint fk_TEAM_Mechanic foreign key ( TMechanic_id) references Mechanic (Mechanic_id),
  constraint fk_TEAM_Service foreign key ( TService_id) references Service (Service_id)
);

CREATE TABLE Order_of_Service (
  OService_id INT auto_increment NOT NULL,
  Date_issuance DATETIME NOT NULL,
  Date_delivery DATETIME,
  OrderValue FLOAT NOT NULL,
  OrderStatus enum('Finalizado','Em processo','Fila') default 'Fila',
  OVehicle_id INT NOT NULL,
  OClient_id INT NOT NULL,
  OrderService_id INT NOT NULL,
  ODescription VARCHAR(45) NULL,
  PRIMARY KEY (OService_id, OVehicle_id, OClient_id,OrderService_id),
  constraint fk_Oservice_Vehicle foreign key ( OVehicle_id) references Vehicle (Vehicle_id),
  constraint fk_Oservice_Client foreign key ( OClient_id) references Clients (Client_id),
  constraint fk_Oservice_OService foreign key ( OrderService_id) references Service (Service_id)
  );
  alter table Order_of_Service auto_increment = 1;

  
  CREATE TABLE Invoice (
  Invoice_id INT auto_increment NOT NULL,
  IValue FLOAT NOT NULL,
  IData DATETIME NOT NULL,
  IDescription VARCHAR(45) NULL,
  IClient_id INT NOT NULL,
  IService_id INT NOT NULL,
  PRIMARY KEY (Invoice_id,IClient_id,IService_id),
  constraint fk_Invoice_Client foreign key ( IClient_id) references Clients (Client_id),
  constraint fk_Invoice_Service foreign key ( IService_id) references Order_of_Service (OService_id)
  );
    alter table Invoice auto_increment = 1;
