---
description: Describes the technical integration for the pay@table use-case.
---

# Pay@Table 1.0.0

## Introduction

Zapper™ for is a mobile application for people to pay a point-of-sale generated order/ check by scanning a customer-facing \(displayed\) QR code using a mobile device. Zapper integrates with a merchant’s point-of-sale \(POS\) to produce the QR code from the basket data. Zapper will deploy a Zebra on site \(hardware that will emulate a network printer\) to intercept the printer data. After injecting the QR code, the Zebra will route the data to the appropriate printer.

This Integration Overview details the pay@table hospitality \(restaurant\) deployment where a relevant Zapper payment QR code is printed on the bill/ check that is tendered for payment at the table of the merchant’s patron.

## Integration Overview

The Micros RES 3700 POS machine integrates with the Zapper back-end payment processing through software middle-ware called the Zapper Event Broker Redundant Adapter \(ZEBRA\).  This software has been renamed in August 2019 to Zapper-Hub \(Zhub\).

A Zhub instance is deployed with connectivity to both the POS network and an Internet gateway. Transaction communications \(with POS\) is brokered by Zhub to create a payment QR code, using the POS item basket for goods purchased by the merchant’s customer. 

Zhub receives the basket data from the Micros POS, via local area network \(LAN\) integration, and renders a QR code for payment to a connected printer.  This is achieved over the network or via the Zapper printer emulation service \(installed on the Micros POS machine, called Ostrich\) to a physically connected serial printer.  Ostrich is a Microsoft .NET service that functions as aprinting emulator.  It services attached serial printers through emulation masquerading as network printers.

> This overview details integrations for pay@table where printing is possible to a network or a directly attached serial printer configuration.

Scanning the QR code, a merchant’s patron, transfers the payment processing data from the printed bill/ check, via their mobile device over the mobile network, to the Zapper cloud server platform for processing. Resultant Zapper processing responses are transmitted back to the Zhub software that notifies the Micros POS as well as back to the Zapper user via the mobile network for similar payment confirmations.

## Constraints & Restrictions

1. Logo printing is not supported if using Zhub to Ostrich solution for serial printers
2. Closure of bill/ check on a Micros RES 3700 POS requires Zapper developed SIM scripts to be installed
3. Logos and banners, although supported \(on the network printer configuration\), do slow down the printing process

## Architecture

The solution for Zapper payment processing integration with Oracle Micros RES 3700 is illustrated by means of the [ArchiMate ](https://en.wikipedia.org/wiki/ArchiMate)notation and [TOGAF ](https://en.wikipedia.org/wiki/The_Open_Group_Architecture_Framework)architecture domains.

### Business Architecture

RES 3700 Pay at Table Business Process View \(Figure 1\) depicts the business process flow for the pay@table \(hospitality/ restaurant\) solution on a Micros RES 3700 integration.

1. Invoice enriching
2. Invoice printing
3. Confirmation receipt printing

The merchant's patron, interacting with the check-out process, experiences the following:

* The merchant’s patron \(customer\) requests the bill/ check from waiting staff at the table of service
* A cashier records the customer’s purchases on the Micros POS by scanning/ selectin items and creating a proforma invoice on total tender
* Micros sends the basket \(invoice details\) to the Zhub middle-ware software for processing
* Zhub transforms the invoice \(basket\) details into a QR code and injects the code onto the bill/ check 
  * **Note**: for integrations with that function enabled in the configuration
* Zhub prints the QR code on the bill/ check, visible to the customer, to initiate payment
* The customer uses a mobile device and scans the QR code to pay the merchant via Zapper processing
* On payment confirmation, Zhub prints a confirmation receipt indicating the status of the payment, viz. success or failure
* The cashier uses the confirmation receipt reference to retrieve/fetch the check on the Micros POS
* The cashier selects the Zapper tender/ payment option to initiate the Micros POS interaction with Zapper
* The Micros POS requests payment confirmation from Zhub
* The open POS order is closed with a successful payment confirmation, in response to the notification processed by the POS machine

### Application Architecture

#### Functional Architecture Views

Zhub exposes application functions to realise all of the services of Zapper payment processing.

The pay@table Application Function View – Ostrich \(Figure 3\) depicts the software printer emulation service \(code-named Ostrich\) to print customer receipts for physically attached serial printers.

#### Service Architecture View

The pay@table Application Service View - SIM \(Figure 4\) depicts the services exposed by the Micros RES 3700 SIM interface. The services are exposed via a TCP/ IP interface with which the Zhub middle-ware interacts. This describes the integration points between Micros RES 3700 and the Zapper payment integration services from the Micros perspective. The realization of these services is described in detail in subsequent diagrams.

#### Application Functional Flows

The pay@table Invoice Sending \(SIM\) Application Process View \(Figure 5\) depicts the application process flow between Micros POS and Zhub. Once the Print Check button is tapped on the POS, invoice details are transmitted via the SIM interface over the TCP/ IP interface to the Zhub component for subsequent processing.

The pay@table Invoice Sending \(Printer\) Application Process View \(Figure 6\) is a continuation of the Invoice Sending \(SIM\) Application Process View. It specifically details the application process flow between the Micros POS and Zapper Zhub. This, in relation to the interception of the print data, the injection of the QR Code, and the routing of the print data to the associated printer.

The pay@table Invoice Network Printing Application Process View \(Figure 7\) is a continuation of the Invoice Sending \(Printer\) Application Process View, specifically in relation to network printer integration.

The pay@table Invoice Serial Printing Application Process View \(Figure 8\) is a continuation of the Invoice Sending \(Printer\) Application Process View, specifically in relation to serial printer integration.

The pay@table Payment Confirmation Processing Application Process View \(Figure 9\) depicts the application process flow between the Zapper Cloud and the Zhub with regards to payment confirmations.

The pay@table Confirmation Requesting Application Process View \(Figure 10\) depicts the application process flow between Micros POS and Zapper Zhub. This is when a request is made for the payment confirmation \(on pressing the Zapper payment tender button on the POS\) by the cashier after payment by the customer.

The pay@table Payment Confirmation Requested Application Process View \(Figure 11\) is a continuation of the Payment Confirmation Requesting Application Process View, specifically with regards the processing of the payment confirmation request/ response.

#### Network Printer Architecture View

RES 3700 Pay at Table Application Usage View - Network Printer \(Figure 12\) depicts application functions executing in fulfillment of the business process for a pay@table \(hospitality/ restaurant\) integration with Micros RES 3700.

In this deployment topology, each Micros POS has a network printer assigned to it prior to the Zhub deployment. Once Zhub is deployed the Micros POS is connected to the Zhub--emulating a network printer--and the Zhub is connected to the associated network printers.

Zhub receives the invoice \(basket\) details from which to create the payment QR code. Zhub creates the payment QR Code to print to a network printer--associated to a Micros POS--where payment is made, by processing the Micros POS basket data.

The merchant’s patron \(customer\) scans the Zapper QR code, using a mobile device, to initiate the Zapper payment processing. The mobile client sends the payment data from the QR code \(combined with the Zapper user’s details\) to the Zapper server cloud for payment processing. A resultant confirmation is sent to Zhub--associated to the originating Micros POS machine with the customer--where the Zhub prints a confirmation receipt. Zapper also transmits a status message to the Zapper user’s mobile device pertaining to the payment processed.

The Micros POS machine obtains the Zapper payment confirmation from Zhub and applies it to the open order.

The pay@table Application Cooperation View - Network Printer \(Figure 13\) depicts the information flow of the component relationships in a Zapper and Micros context.

The Micros POS application sends invoice details to the Zapper Zhub middle-ware software component. The Zhub generated QR code is printed to a network printer and is compiled from the POS generated basket/ invoice bill-item details.

The rendered QR code \(invoice\) serves the mobile client, utilizing the Zapper Mobile Payment Application, to initiate the payment process. Zapper processes the customer’s payment and sends payment confirmations to both the customer and the Zhub component.

The Zhub prints a payment confirmation receipt to the configured network printer. The Micros POS application requests the payment confirmations \(via SIM\) and processes those against relative open orders.

The pay@table Payment Confirmation Processing Application Process View \(Figure 14\) depicts the application process flow between the Zapper Cloud and the Zhub with regards to payment confirmations.

Zhub middle-ware software is deployed on a computing device \(ranging from a single-board computer up to a cloud hosted server\) where the Micros POS infrastructure uses the local area network \(LAN\) and a network attached printer. Micros POS, the LAN printer, and Zhub interact with each other over the LAN and with the Zapper payment platform over the Internet gateway or mobile network.

#### Serial Printer Architecture View

RES 3700 Pay at Table Application Usage View - Serial Printer \(Figure 16\) depicts application functions executing in fulfillment of the business process for a pay@table \(hospitality/ restaurant\) integration with Micros RES 3700.

In this deployment topology, each Micros POS machine is connected to a physically attached serial printer. Once Zhub is deployed, each Micros POS is reconfigured to use the Zhub as a network printer.

Zhub receives the invoice \(basket\) details from which to create the QR payment code. ZEBRA creates the payment QR Code to print to a printer \(associated to a Micros POS\) where payment is made by processing the Micros POS basket data.

The merchant’s customer scans the Zapper QR code, using a mobile device, to initiate the Zapper payment processing. The mobile client sends the payment data from the QR code \(combined with the Zapper user’s details\) to the Zapper server cloud for payment processing. A resultant confirmation is sent to Zhub associated to the originating Micros POS machine with the customer.

To cater for the physically attached serial printer configuration, Zhub routes the print data to the Zapper print emulation service--on the Micros POS machine--to emulate printing to a network printer.

> The printer emulation service \(Ostrich\) ensures printing for the physically attached serial printers through network printer emulation and prints the confirmation receipt.

The Micros POS machine obtains the Zapper payment confirmation from Zhub and applies it to the open order.

The pay@table Application Cooperation View - Serial Printer \(Figure 17\) depicts the information flow and integration points for components within the context of Zapper and Micros for this integration.

 A Micros RES POS--with physically attached serial printer \(Figure 17\)--utilizes a software printer emulator service \(Ostrich\), installed on the Micros POS machine, to facilitate serial printing. The POS application sends invoice details to the Zapper Zhub middle-ware software component. 

The Zhub generated QR code is rendered/printed on check, compiled from the POS generated basket/ invoice bill-item details. The printed QR code \(invoice/ bill/ check\) serves the mobile client, utilizing the Zapper Mobile Payment Application, to initiate the payment process. 

Zapper processes the customer’s payment and sends payment confirmations to both the customer and the Zhub component. Zhub prints a payment confirmation receipt. 

The receipt is conveyed via the software printer emulation software service on the POS machine, routing the receipt to the physically attached serial printers like those are network attached devices. The Micros POS application requests for payment confirmations \(via SIM\) and processes those against relative open orders.

The pay@table Confirmation Receipt Serial Printing Application Process View \(Figure 18\) is a continuation of the Confirmation Processing Application Process View, specifically with regards to a physically attached serial printer integration.

Zhub middle-ware software is deployed on a computing device \(ranging from a single-board computer up to a cloud hosted server\) where the Micros POS infrastructure uses the local area network \(LAN\) and a network attached printer. Micros POS, the LAN printer, and Zhub interact with each other over the LAN and with the Zapper payment platform over the Internet gateway or mobile network.



