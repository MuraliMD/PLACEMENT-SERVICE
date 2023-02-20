1. PlacementserviceApplication.java

package com.tns.Placementservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class PlacementserviceApplication
{
	public static void main(String[] args)
	{
		SpringApplication.run(PlacementserviceApplication.class, args);
	}
}


2.Placementservice.java

package com.tns.Placementservice;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Placementservice
{
	private Integer S_id;
	private String S_name;
	
	
	public Placementservice() 
	{
		super();
	}
	
	public Placementservice(Integer s_id, String s_name)
	{
		super();
		S_id = s_id;
		S_name = s_name;
	}
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	public Integer getS_id() {
		return S_id;
	}
	public void setS_id(Integer s_id) {
		S_id = s_id;
	}
	public String getS_name() {
		return S_name;
	}
	public void setS_name(String s_name)
	{
		S_name = s_name;
	}
	@Override
	public String toString()
	{
		return "Student[Student id:"+S_id+" Student name"+S_name+"]";
	}
}


3.Placement_Service_Repository.java 
package com.tns.Placementservice;

import org.springframework.data.jpa.repository.JpaRepository;

public interface Placement_Service_Repository extends JpaRepository<Student, Integer> 
{

}


4. Placement_Service.java 

package com.tns.Placementservice;

import java.util.List;

import javax.transaction.Transactional;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
@Service
@Transactional
public class Student_Service 
{
	@Autowired
	private Student_Service_Repository repo;
	
	public List<Student> listAll()
	{
		return repo.findAll();
	}
	
	public void save(Student stud)
	{
		repo.save(stud);
	}
	
	public Student get(Integer s_id)
	{
		return repo.findById(s_id).get();
	}
	public void delete(Integer s_id)
	{
		repo.deleteById(s_id);
	}
	
}


5. Placement_service_Controller.java (controller)

package com.tns.Placementservice;

import javax.persistence.NoResultException;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class Placement_service_Controller
{
	@Autowired(required=true)
	private Placement_Service service;
	
	@GetMapping("/placementservice")
	public java.util.List<Placement> list()
	{
		return service.listAll();
	}
	
	@GetMapping("/placementservice/{s_id}")
	public ResponseEntity<Placement> get(@PathVariable Integer S_id)
	{
		try
		{
			Student stud=service.get(S_id);
			return new ResponseEntity<Placement>(stud,HttpStatus.OK);
		}
		catch(NoResultException e)
		{
			return new ResponseEntity<Placement>(HttpStatus.NOT_FOUND);
		}
	}
	@PostMapping("/Placementservice")
	public void add(@RequestBody Placement stud)
	{
		service.save(stud);
	}
	
	@PutMapping("/Placementservice/{s_id}")
	public ResponseEntity<?> update(@RequestBody Placement stud, @PathVariable Integer S_id)
	{
		Placement existstud=service.get(S_id);
		service.save(existstud);
		return new ResponseEntity<>(HttpStatus.OK);
	}
	
	@DeleteMapping("/placementservice/{s_id}")
	public void delete(@PathVariable Integer s_id)
	{
		service.delete(s_id);
	}
}
-------------------------------------------------------------------------------

Go to src/resource folder

-application.properties

spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://localhost:3306/studentservice
spring.datasource.username=root
spring.datasource.password=Tamil@123
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5InnoDBDialect
server.port=8080









