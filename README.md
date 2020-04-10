package com.test.xml;

import java.io.File;
import java.io.IOException;
import java.io.StringWriter;

import javax.xml.parsers.*;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import org.w3c.dom.*;
import org.xml.sax.SAXException;

public class XMLDataDemo {
	
	//static File file = new File("D:\\dev\\test\\xmlMACD\\data\\cuihua.xml");
	
	public static int countFile() {
		File[] list = new File("D:\\dev\\test\\xmlMACD\\data").listFiles();
		int fileCount = 0;
		
		for(File file : list) {
			if(file.isFile()) {
				fileCount++;
			}
		}
		return fileCount;
	}
	
	public String setFileName() {
		int counter = countFile() + 1;
		return "xml" + Integer.toString(counter);
	}
	
	public void addXML(String name , String age, String sex) {
		 DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
	     DocumentBuilder builder;
	     int id = countFile() + 1;
	        try {
	            builder = documentBuilderFactory.newDocumentBuilder();
	            Document document = builder.newDocument();
	            Element root = document.createElement("person");
	            root.setAttribute("id", Integer.toString(id));

	            Element name1 = document.createElement("name");
	            name1.setTextContent(name);
	            Element age1 = document.createElement("age");
	            age1.setTextContent(age);
	            Element sex1 = document.createElement("sex");
	            sex1.setTextContent(sex);
	            
	            
	            root.appendChild(name1);
	            root.appendChild(age1);
	            root.appendChild(sex1);
	            
	            document.appendChild(root);
	            
	            TransformerFactory transformerFactory = TransformerFactory.newInstance();
	            Transformer transformer = transformerFactory.newTransformer();
	            transformer.transform(new DOMSource(document), new StreamResult(new File("D:\\dev\\test\\xmlMACD\\data\\"+setFileName()+".xml")));
	            

	            StringWriter stringWriter = new StringWriter();
	            transformer.transform(new DOMSource(document), new StreamResult(stringWriter));
	            System.out.println(stringWriter.toString());
	            
	        } catch (Exception e) {
	            e.printStackTrace();
	        }
	}
	
	public void select(String fileName) throws Exception {
		String filePath = "D:\\dev\\test\\xmlMACD\\data\\";
		File file = new File(filePath + fileName);
		
		DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
		DocumentBuilder db = dbf.newDocumentBuilder();
		Document document = db.parse(file);
		
		NodeList nl = document.getElementsByTagName("person");
		System.out.println("total length = " + nl.getLength());
		for(int i = 0 ; i < nl.getLength(); i++) {
			Node node1 = nl.item(i);
			Element element = (Element) nl.item(i);
			System.out.println("person id = "+element.getAttribute("id"));
			NodeList childNodes = node1.getChildNodes();
			System.out.println("Children node are total = " + childNodes.getLength());
			for(int j = 0 ; j < childNodes.getLength(); j++) {
				System.out.println( childNodes.item(j).getNodeName() + " = " + childNodes.item(j).getTextContent());
			}
		}
	}
	
	public void update(String fileName, String name, String age, String sex) throws Exception{
		String filePath = "D:\\dev\\test\\xmlMACD\\data\\";
		File file = new File(filePath + fileName);
		
		DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
		DocumentBuilder db = dbf.newDocumentBuilder();
		Document document = db.parse(file);
		
		NodeList nl = document.getElementsByTagName("person");
		System.out.println("total length = " + nl.getLength());
		for(int i = 0 ; i < nl.getLength(); i++) {
			Node node1 = nl.item(i);
			Element element = (Element) nl.item(i);
			System.out.println("person id = "+element.getAttribute("id"));
			NodeList childNodes = node1.getChildNodes();
			System.out.println("Children node are total = " + childNodes.getLength());
			for(int j = 0 ; j < childNodes.getLength(); j++) {
				if(childNodes.item(j).getNodeName() == "name") {
					childNodes.item(j).setTextContent(name);
				}
				if(childNodes.item(j).getNodeName() == "age") {
					childNodes.item(j).setTextContent(age);
				}
				if(childNodes.item(j).getNodeName() == "sex") {
					childNodes.item(j).setTextContent(sex);
				}
				
				System.out.println( childNodes.item(j).getNodeName() + " = " + childNodes.item(j).getTextContent());
			}
		}
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
        Transformer transformer = transformerFactory.newTransformer();
        transformer.transform(new DOMSource(document), new StreamResult(file));
        

        StringWriter stringWriter = new StringWriter();
        transformer.transform(new DOMSource(document), new StreamResult(stringWriter));
	}
	
	public void delete(String fileName) {
		String filePath = "D:\\dev\\test\\xmlMACD\\data\\";
		File file = new File(filePath + fileName);
		if(file.exists() && file.isFile()) {
			file.delete();
			System.out.println(fileName + " is deleted! ");
		}else {
			System.out.println("Nothing to delete! ");
		}
	}
	
	
	
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		XMLDataDemo demo1 = new XMLDataDemo();

		try {
			demo1.update("xml1.xml","superman","29","male");
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		

	}

}
