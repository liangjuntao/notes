```  	
<%@ page import="java.security.cert.X509Certificate"%>  
<%@ page import="sun.security.x509.X500Name"%>  
  
<%  
	String certSubject = null;
	java.security.cert.X509Certificate[] certChain = (java.security.cert.X509Certificate[]) request
			.getAttribute("javax.servlet.request.X509Certificate");
	if (null != certChain) {
		int len = certChain.length;
		if (len > 0) {
			java.security.cert.X509Certificate cert = (java.security.cert.X509Certificate) certChain[0];
			javax.security.auth.x500.X500Principal pSubject = cert.getSubjectX500Principal();
			X500Name x500Name = new X500Name(pSubject.getName());
			if(x500Name.getCommonName() != null && "".equals(x500Name.getCommonName())){
				window.location.href = "index";				
			}
		}
	}
%>  
```