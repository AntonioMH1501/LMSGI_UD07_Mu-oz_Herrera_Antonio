# Manual de Explotación: Sistema ERP/CRM WillmanTech


## 1. Introducción y Arquitectura del Sistema
El sistema de gestión de "WillmanTech S.L." está diseñado para optimizar la venta y facturación de los productos. La arquitectura se basa en una infraestructura Docker que garantiza la portabilidad y escalabilidad de los servicios.

## DOCX

Módulos Activos: Gestión de Ventas, Facturación y CRM.
Topología Lógica: El despliegue se realiza mediante Docker Compose, a través de contenedores para la aplicación (Odoo/ERP), la base de datos (PostgreSQL) y el motor de informes.

## 2. Guía de Instalación y Reinstalación
Para levantar el entorno de explotación desde cero, habrá que lelvar a cabo los siguientes pasos:
Requisitos previos: Tener instalado Docker y Docker Compose.

Configuración: Definir las variables de entorno en el archivo .env (USER, PASSWORD, DB_NAME).

Despliegue: Ejecutar el comando:

docker-compose up -d


## Dependencias: El sistema requiere la imagen oficial de PostgreSQL como Sistema Gestor de Base de Datos (SGBD).

## 3. Seguridad y Control de Acceso
La integridad de la información de "WillmanTech S.L." se gestiona mediante una política de privilegios mínimos:
Roles Definidos:
Administrador: acceso total a la configuración y logs del sistema.
Contable: permisos para validar facturas y exportar datos fiscales.
Comercial: gestión de presupuestos y clientes.
Políticas: se aplican reglas de complejidad de contraseñas y auditoría de accesos sobre la información sensible.

## 4. Procedimientos de Backup y Restauración
Para garantizar la sostenibilidad operativa, se deben programar respaldos periódicos:

Copia de Seguridad: Utilizar el comando de volcado de la base de datos relacional:

docker exec -t db_container pg_dump -U odoo > backup_willmantech.sql


## Restauración: Limpiar la base de datos de destino y ejecutar el script SQL generado.

## 5. Flujo Operativo de Facturación e Informes
Creación: el usuario registra la transacción en el ERP.
Renderizado (Pipeline):
-El motor QWeb procesa la plantilla XML (report_invoice_willmantech.xml) inyectando los datos de la base de datos.
-El resultado HTML se envía a la herramienta wkhtmltopdf.
-Se genera el archivo PDF final que se entrega al cliente.
