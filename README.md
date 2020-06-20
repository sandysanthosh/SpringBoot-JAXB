# JAVA-JAXB

Java to xml binding


#### Maven plugin:

       <groupId>org.codehaus.mojo</groupId>
       <artifactId>jaxb2-maven-plugin</artifactId>
        <version>1.6</version>
                  
#### Student POJO:

        public class Student {
          private Long id;
          private String name;
          private String passportNumber;
          }
          
#### Endpoint:



          @Endpoint
          public class StudentDetailsEndpoint {

            @PayloadRoot(namespace = "http://in28minutes.com/students", localPart = "GetStudentDetailsRequest")
            @ResponsePayload
            public GetStudentDetailsResponse processCourseDetailsRequest(@RequestPayload GetStudentDetailsRequest request) {
              GetStudentDetailsResponse response = new GetStudentDetailsResponse();

              StudentDetails studentDetails = new StudentDetails();
              studentDetails.setId(request.getId());
              studentDetails.setName("Adam");
              studentDetails.setPassportNumber("E1234567");

              response.setStudentDetails(studentDetails);

              return response;
            }

          }
          
#### WebServiceConfig:

      @EnableWs
      @Configuration
      public class WebServiceConfig {

        @Bean
        public ServletRegistrationBean messageDispatcherServlet(ApplicationContext context) {
          MessageDispatcherServlet messageDispatcherServlet = new MessageDispatcherServlet();
          messageDispatcherServlet.setApplicationContext(context);
          messageDispatcherServlet.setTransformWsdlLocations(true);
          return new ServletRegistrationBean(messageDispatcherServlet, "/ws/*");
        }

        @Bean(name = "students")
        public DefaultWsdl11Definition defaultWsdl11Definition(XsdSchema studentsSchema) {
          DefaultWsdl11Definition definition = new DefaultWsdl11Definition();
          definition.setPortTypeName("StudentPort");
          definition.setTargetNamespace("http://in28minutes.com/students");
          definition.setLocationUri("/ws");
          definition.setSchema(studentsSchema);
          return definition;
        }

        @Bean
        public XsdSchema studentsSchema() {
          return new SimpleXsdSchema(new ClassPathResource("student-details.xsd"));
        }
      }
  
  
#### /src/main/resources/student-details.xsd


        <?xml version="1.0" encoding="UTF-8"?>

      <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" 
      targetNamespace="http://in28minutes.com/students" 
      xmlns:tns="http://in28minutes.com/students" elementFormDefault="qualified">

        <xs:element name="GetStudentDetailsRequest">
          <xs:complexType>
            <xs:sequence>
              <xs:element name= "id" type="xs:int"/>
            </xs:sequence>	
          </xs:complexType>
        </xs:element>

        <xs:element name="GetStudentDetailsResponse">
          <xs:complexType>
            <xs:sequence>
              <xs:element name= "StudentDetails" type="tns:StudentDetails"/>
            </xs:sequence>	
          </xs:complexType>
        </xs:element>

        <xs:complexType name="StudentDetails">
          <xs:sequence>
            <xs:element name="id" type="xs:int"/>
            <xs:element name="name" type="xs:string"/>
            <xs:element name="passportNumber" type="xs:string"/>
          </xs:sequence>
        </xs:complexType>
    </xs:schema>
	
  
  
