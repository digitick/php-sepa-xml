php-sepa-xml
============

SEPA file generator for PHP.

Creates an XML file for a Single Euro Payments Area (SEPA) Credit Transfer.

The version of the standard followed is: _pain.001.001.03_

License: GNU Lesser General Public License v3.0

BETA QUALITY SOFTWARE

Verify generated files with your bank before using!!!

##Installation
###Composer
This library is available in packagist.org, you can add it to your project
via Composer.

In the "require" section of your composer.json file:

Always up to date (bleeding edge, API *not* guaranteed stable)
```javascript
"digitick/sepa-xml" : "dev-master"
```
Specific minor version, API stability
```javascript
"digitick/sepa-xml" : "0.9.*"
```

##Usage
```php
$sepaFile = new SepaTransferFile();
$sepaFile->messageIdentification = 'transferID';
$sepaFile->initiatingPartyName = 'Me';

/* 
 * Add a payment to the SEPA file. This method
 * may be called more than once to add multiple
 * payments to the same file.
 */
$payment1 = $sepaFile->addPaymentInfo(array(
	'id'					=> 'Payment Info ID',
	'debtorName'			=> 'My Corp',
	'debtorAccountIBAN'		=> 'MY_ACCOUNT_IBAN',
	'debtorAgentBIC'		=> 'MY_BANK_BIC'
//	'debtorAccountCurrency'	=> 'GPB', // optional, defaults to 'EUR'
//	'categoryPurposeCode'	=> 'SUPP', // optional, defaults to NULL
//	'requestedExecutionDate => '2014-1-28', // optional, defaults to current date
));

/* 
 * Add a credit transfer to the payment. This method
 * may be called more than once to add multiple
 * transfers for the same payment.
 */
$payment1->addCreditTransfer(array(
	'id'					=> 'Id shown in bank statement',
	'currency'				=> 'EUR',
	'amount'				=> '0.02', // or as float: 0.02 or as integer: 2
	'creditorBIC'			=> 'THEIR_BANK_BIC',
	'creditorName'			=> 'THEIR_NAME',
	'creditorAccountIBAN'	=> 'THEIR_IBAN',
	'remittanceInformation'	=> 'Transaction description',
));

/* Generate the file and return the XML string. */
echo $sepaFile->asXML();

/* After generating the file, these two values can be retrieved: */
echo $sepaFile->getHeaderControlSumCents();
echo $payment1->getPaymentControlSumCents();
```
