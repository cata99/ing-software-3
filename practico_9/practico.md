1- Utilizando Unit test
¿En el proyecto spring-boot para qué está esta dependencia en el pom.xml?
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
Analizar y ejectuar el metodo de unit test:
public class HelloWorldServiceTest {

	@Test
	public void expectedMessage() {
		HelloWorldService helloWorldService = new HelloWorldService();
		assertEquals("Expected correct message","Spring boot says hello from a Docker container",helloWorldService.getHelloMessage());
	}
	
}
![Screenshot from 2021-11-15 17-54-01](https://user-images.githubusercontent.com/48757813/141852364-61786539-5c8b-4f14-8274-40660031aa4c.png)


Ejecutar los tests utilizando la IDE
3- Familiarizarse con algunos conceptos de Mockito
Mockito es un framework de simulación popular que se puede usar junto con JUnit. Mockito permite crear y configurar objetos falsos. El uso de Mockito simplifica significativamente el desarrollo de pruebas para clases con dependencias externas.

Si se usa Mockito en las pruebas, normalmente:

Se burlan las dependencias externas e insertan los mocks en el código bajo prueba
Se ejecuta el código bajo prueba
Se valida que el código se ejecutó correctamente
Referencia: https://www.vogella.com/tutorials/Mockito/article.html

Analizar el código del test
public class ExampleInfoContributorTest {

	@Test
	public void infoMap() {
		Info.Builder builder = mock(Info.Builder.class);
		
		ExampleInfoContributor exampleInfoContributor = new ExampleInfoContributor();
		exampleInfoContributor.contribute(builder);
		
		verify(builder).withDetail(any(),any());
	}
}

TEST REALIZADOS: 

![Screenshot from 2021-11-15 17-42-46](https://user-images.githubusercontent.com/48757813/141852421-490da3fa-007e-4b18-830c-0249874b0920.png)
![Screenshot from 2021-11-15 17-09-29](https://user-images.githubusercontent.com/48757813/141852426-cecc4478-5419-4156-85d5-e2d7d31ea0ae.png)


4- Utilizando Mocks //No los pude hacer porque falle miserablemente
Agregar un unit test a la clase HelloWorldServiceTest

Cuando se llame por primera vez al método getHelloMessage retorne "Hola Hola"
Cuando se llame por segunda vez al método getHelloMessage retorne "Hello Hello"
Crear la siguiente clase AbstractTest

package sample.actuator;

import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = SampleActuatorApplication.class)
@WebAppConfiguration
public abstract class AbstractTest {
    protected MockMvc mvc;

    @Autowired
    WebApplicationContext webApplicationContext;

    protected void setUp() {
        mvc = MockMvcBuilders.webAppContextSetup(webApplicationContext).build();
    }
}
Agregar esta otra clase también en el mismo directorio
package sample.actuator;

import static org.junit.Assert.assertEquals;

import org.junit.Before;
import org.junit.Test;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MvcResult;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;


public class SampleControllerTest extends AbstractTest {

    @Override
    @Before
    public void setUp() {
        super.setUp();
    }

    @Test
    public void testRootMessage() throws Exception {
        String uri = "/";
        MvcResult mvcResult = mvc.perform(MockMvcRequestBuilders.get(uri)
                .accept( MediaType.APPLICATION_JSON_VALUE)).andReturn();

        String content = mvcResult.getResponse().getContentAsString();
        int status = mvcResult.getResponse().getStatus();
        assertEquals(200, status);
        assertEquals("Expected correct message","{\"message\":\"Spring boot says hello from a Docker container\"}",content);
    }
}
