# CA-Student-Info-System
Developed a student information system for the unique processes of a school primarily focused on a different approach to teaching, for obvious reasons couldn't put up the actual codebase, but have included selective code samples of my style at the time...good or bad, probably mostly bad :), and some screenshots showing examples of my work and listed the functionality developed.

Highlights of technologies used: Used MSSQL Server 2015 (database), pojo Java/Hibernate (model), Spring Framework (data access/services/web controllers/security/etc), Aspects (audit-logging), jsp/jquery/jstl/bootstrap (frontend), developed using Eclipse/STS, deployed on Tomcat server, utilizing Maven, Git repository...following the MVC pattern. Tried to follow the Don't Repeat Yourself principle and liked Keep It Simple S and self documenting code

Last project was to become compliant with Ab Ed Using their Web Services via web services definition language files.  Used Axis to generate boilerplate code based on those wsdl files and utilized long polling to keep data syncd in near real time.

https://extranet.education.alberta.ca/pasidevnet/Docs/

Wanted to finish PASI compliance (sadly failed to finish for various reasons), move from a system that was becoming a monolithic bloated one, to using a RESTful api for the backend and other modern Java/Spring features, like Java Modules, Spring Boot and so much syntactic sugar making is so much more maintainable/extendible.  Even just on the frontend client side switching from js callback hell to try/catch with await/async or at least returned promises using the .then/.catch which to me is much more readable. Mainly used jquery upfront, tried to update using template engines like Thymeleaf, AngularJS was just becoming more mainstream. 


1. [Screenshots](#Screenshots)
2. [Code Samples](#Code_Samples)
3. [A little about me](#about)



## Screenshots

### Homepage
![homepage](https://github.com/jaysait/CA-Student-Info-System/blob/master/homepage.png)
- customizable calendar feed
- classes for attendance, grades, rubrics, etc
- notifications about issues in the system ie. duplicate enrollments in subjects
- many reports, either in pdf via iText or spreadsheet via Apache Poi, etc

### Homepage 2
![homepage2](https://github.com/jaysait/CA-Student-Info-System/blob/master/homepage-2.png)
- quick lookups 
- parent teacher scheduling functionality
- online options allowing parents to pick options for the next year
- reenrollment functionality
- manage various aspects of the system

### Edit Profile
![Edit Profile](https://github.com/jaysait/CA-Student-Info-System/blob/master/edit-profile.png)
- used bootstrap for interface

### Yearend Recognition Awards
![Yearend Recognition Awards](https://github.com/jaysait/CA-Student-Info-System/blob/master/yearend-recognition-awards.png)
- used bootstrap for interface

### Yearend Recognition Awards - Customize Awards
![Yearend Recognition Awards - Customize Awards](https://github.com/jaysait/CA-Student-Info-System/blob/master/yearend-recognition-awards-custom.png)

### Yearend Recognition Awards - Student Awards
![Yearend Recognition Awards - Student Awards](https://github.com/jaysait/CA-Student-Info-System/blob/master/yearend-recognition-awards-per-student.png)

### Yearend Recognition Awards - Export
![homepage](https://github.com/jaysait/CA-Student-Info-System/blob/master/yearend-recognition-awards-export.png)
- allowed graphic person to export awards and mail merge into the actual award documents

### Individual Program Plan
![Yearend Recognition Awards - Export](https://github.com/jaysait/CA-Student-Info-System/blob/master/Individual-program-plan-main.png)
- used jquery ui for frontend


### Individual Program Plan - Goals
![Individual Program Plan - Goals](https://github.com/jaysait/CA-Student-Info-System/blob/master/Individual-program-plan-goals.png)

### Individual Program Plan - Edit Goals
![Individual Program Plan - Edit Goals](https://github.com/jaysait/CA-Student-Info-System/blob/master/Individual-program-plan-edit-goals.png)

### PT Interview Scheduler
![PT Interview Scheduler](https://github.com/jaysait/CA-Student-Info-System/blob/master/ptinterview.png)
-Teachers can dynamically create PT Interviews for any of their classes, allowing to set up a template email, pick available times, select who gets invited and share with other teachers and view/get notified of any selections


### Subscriptions to Reports
![Individual Program Plan - Edit Goals](https://github.com/jaysait/CA-Student-Info-System/blob/master/subscriptions.png)
- allowed teachers/directors to set criteria to receive automatic weekly reports (ie grades below a certain %)

### Create Class
![Create Class](https://github.com/jaysait/CA-Student-Info-System/blob/master/create-class.png)

### Attendance
![Attendance](https://github.com/jaysait/CA-Student-Info-System/blob/master/attendance.png)

### Gradebook
![Gradebook](https://github.com/jaysait/CA-Student-Info-System/blob/master/gradebook.png)
- autosave
- right click meta data

### Export Data
![Export Data](https://github.com/jaysait/CA-Student-Info-System/blob/master/export-data.png)
- could autosuggest and export (spreadsheet) much of the data in the system to be used externally

### Mail Merge
![mail-merge-1](https://github.com/jaysait/CA-Student-Info-System/blob/master/mail-merge-1.png)
- users could create a .docx document and mark it with special delimited fields ${} and upload it into the system and it would find them and allow them to match them with data in the system ie. student name, gender, etc
-  basically a mail merge functionality built in using system data and without having to know how to mail merge in MS

![mail-merge-2](https://github.com/jaysait/CA-Student-Info-System/blob/master/mail-merge-2.png)

![mail-merge-3](https://github.com/jaysait/CA-Student-Info-System/blob/master/mail-merge-3.png)
- then they could apply that for any grade

![mail-merge-4](https://github.com/jaysait/CA-Student-Info-System/blob/master/mail-merge-4.png)
- and the system would generate individual documents for each of the kids with the appropriate data filled in

### Labels
![Labels](https://github.com/jaysait/CA-Student-Info-System/blob/master/address-labels.png)
- allowed flexible labels for various tasks ie address

### Various Stats
![Various Stats](https://github.com/jaysait/CA-Student-Info-System/blob/master/login-stats.png)
- had many different metrics/stats to try and help analyse issues

### Permissions
![Permissions](https://github.com/jaysait/CA-Student-Info-System/blob/master/permissions.png)
- finegrain permissions allowing permitted users to control what groups or individuals saw in the system




## Code Samples <a name="Code_Samples"></a>

```java
@Entity
@Table(name = "reach_outcome")
public class ReachOutcome {
	@Id
	@GeneratedValue(strategy = IDENTITY)
	private Integer id;
	@ManyToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "reach_heading_id")
	private ReachHeading heading;
	@Column(name = "name")
	private String name;
	
	@Column(name = "display_order")
	private Integer order;
...
```
-Used Hibernate to annotate pojo classes and relationships, validations, etc

```java
public class IPPJdbcDaoImpl implements IPPDao {
	private JdbcTemplate jdbcTemplate;
	public void setDataSource(DataSource dataSource) {
		this.jdbcTemplate = new JdbcTemplate(dataSource);
	}
	
	public synchronized void saveSubjects(Set<String> subjects, String scid){
	    
		for(String subject : subjects){
			this.jdbcTemplate.update("INSERT INTO ipp_subjects ( name, school_id)  VALUES ( ?,?)",
						new Object[] { subject, scid});
		}
	  
	}
	
public synchronized int saveSubject(String subject, String scid){
	    
	 KeyHolder keyHolder = new GeneratedKeyHolder();
	 final String subjectF = subject;
	 final String scidF = scid;
	 jdbcTemplate.update(
	            new PreparedStatementCreator() {
	                public PreparedStatement createPreparedStatement(Connection connection) throws SQLException {
	                	String sql ="INSERT INTO ipp_subjects ( name, school_id)  VALUES ( ?,?)";
	                    	PreparedStatement ps = connection.prepareStatement(sql, new String[] {"id"});
	                    	ps.setString(1, subjectF);
	                    	ps.setString(2, scidF);
	                    	return ps;
	                }
	            },
	            keyHolder);
		return keyHolder.getKey().intValue();
	}	
	
public Map<String,List<SchoolHistory>> getSchoolHistories(String scid) {
	
	return (Map<String, List<SchoolHistory>>) this.jdbcTemplate.query(
		"Select * from ipp_school_history where school_id = ? order by student_id, display_order",
		new Object[] {scid },
		new ResultSetExtractor() {
			public Object extractData(ResultSet rs) throws SQLException {
				Map<String, List<SchoolHistory>> map = new HashMap<String, List<SchoolHistory>>();
				while (rs.next()) {
					SchoolHistory history = new SchoolHistory();
					history.setId(rs.getInt("id"));
					history.setOrder(rs.getInt("display_order"));
					history.setSchool(rs.getString("school"));
					history.setStudentId(rs.getInt("student_id"));
					history.setYear(rs.getString("year"));
					history.setSchoolId(rs.getString("school_id"));
					if(map.containsKey(history.getStudentId()+"")){
						map.get(history.getStudentId()+"").add(history);
					}else{
						List<SchoolHistory> hists = new ArrayList<SchoolHistory>();
						hists.add(history);
						map.put(history.getStudentId()+"", hists);
					}
	
				}
				return map;
				};
			});
}

```
-Data Access Object/Repository Files:  Initially when switching over to Spring, used JdbcTemplate w/ rowmappers, etc...was in the process of updating all to use Hibernate/JPA and other modern Spring techniques
-Wanted to use latest things liked Lambdas, other current Java features. 

```java
public List<ReachHeading> getReachBySchool(Integer schoolId) {

	List<ReachHeading> reachs = sessionFactory.getCurrentSession().createCriteria(ReachHeading.class, "reach")
				.setFetchMode("outcomes", FetchMode.JOIN)
				.setResultTransformer(Criteria.DISTINCT_ROOT_ENTITY)
				.add(Restrictions.eq("schoolId", schoolId))
				.addOrder(Order.asc("order"))
				.list();
				
	return reachs;

}
```
-DAO using hibernate example, was excited to try to upgrade to more modern techs from Spring like 'extends CrudRepository<User, Long>' using Spring Data JPA, etc further 
-Also used a service layer above the DAO layer for the main code that would be used to perform business logic, utilizing Spring transactions, etc

```java
public class CourseInfoServlet extends javax.servlet.http.HttpServlet implements javax.servlet.Servlet {
	   
	public CourseInfoServlet() {
		super();
	}   	
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doPost(request,response);
	}  		
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	     	CourseCategoryDao courseCategoryDao=AppCon.getCourseCategoryDao();
	    	RubricDao rubricDao=AppCon.getRubricDao();
		HttpSession session = request.getSession(true);
		String scid = request.getParameter("scid");
		String usercommand = request.getParameter("mancat");
       
        if(usercommand.equalsIgnoreCase("find")){
                String grade = request.getParameter("grade").trim();
                String subject = request.getParameter("subject").trim();
                List<CourseCategoryDefaults> ccd = courseCategoryDao.getCourseCategoryDefaults(subject,grade,scid);
                if(ccd.size()==0){
                	session.removeAttribute("managecategory");
                	List<CourseCategoryDefaults> tmpccd = new ArrayList<CourseCategoryDefaults>();
                	CourseCategoryDefaults tccd = new CourseCategoryDefaults();
                	tccd.setGrade(Integer.parseInt(grade));
                	tccd.setGroup(Integer.parseInt(subject));
                	tccd.setCategory("Enter category Name");
                	tccd.setPercent(0);
                	tccd.setSchoolId(scid);
                	tccd.setOrder(1);
                	tmpccd.add(tccd);
                	session.setAttribute("managecategory", tmpccd);
                	
                	session.setAttribute("managecategorystatus","No categories for that subject/grade.");
                }else{
                	session.setAttribute("managecategory", ccd);  
                }
...
 	response.sendRedirect("managecourseinfo.jsp"+"?scid="+scid);
```
-started out by using servlets for the controller level of MVC, but then was switching over to web @Controller classes  

```java
@Controller
@RequestMapping(value="/ipp")
public class IPPController {

	@Autowired
	private IPPDao ippDao;
	@Autowired
	private TermDao termDao;
	@Autowired
	private StudentDao studentDao;
...
	@RequestMapping(value="/ipp/{courseId}/{schoolId}", method=RequestMethod.GET)
	public String getAvailability(@PathVariable String courseId, @PathVariable String schoolId,HttpServletRequest request, Model model) {
		HttpSession session = request.getSession(true);
		Teacher user =  (Teacher)session.getAttribute("user");
		if(user == null){
			String us = loginDao.getUserBySession(session.getId(), schoolId);
			user = teacherDao.getTeacherInfo(us, schoolId);
		}
		model.addAttribute("scid", schoolId);
		if(user == null){
			return "login";
		}
		model.addAttribute("saying", Validate.getSaying());
		
		String term = termDao.getTermCode(schoolId);
		
		List<Student> students = studentDao.getStudentsByCourseListWithCourses(courseId, term, schoolId);
		String endyear = optionsDao.getName("endyear", schoolId);
		model.addAttribute("endyear", endyear);
		String startyear = optionsDao.getName("startyear", schoolId);
		model.addAttribute("startyear", startyear);
		Map<String,List<PrincipleDean>> principles = ippDao.getPrincipalDeans(schoolId);
		model.addAttribute("principles", principles);
		Map<String,List<Accommodation>> accommodations = ippDao.getAccommodations(schoolId);
		model.addAttribute("accommodations", accommodations);
		List<Teacher> teachers = teacherDao.getTeachers(schoolId, "enabled");
		
...
				
		model.addAttribute("schools", schools);
		
		model.addAttribute("teachers", teachers);
		Gson gson = new Gson();
		List<GsonIdName> gs = new ArrayList<GsonIdName>();
		for(Teacher teacher : teachers){
			GsonIdName gi = new GsonIdName();
			gi.setId(teacher.getId());
			gi.setText(teacher.getFirstName()+" "+teacher.getLastName());
			gs.add(gi);
		}
		model.addAttribute("gs", gson.toJson(gs));
 
		String existingTheme = ippDao.getTheme(user.getId(), schoolId);
		if(existingTheme.equals("")){
		 	existingTheme = "flick";
		}
		model.addAttribute("existingTheme", existingTheme);
		model.addAttribute("path_check", optionsDao.getPath("reportcheck",schoolId));
		model.addAttribute("path_link", optionsDao.getPath("reportlink",schoolId));
		model.addAttribute("slash", "/");
		model.addAttribute("ipp_word", "ipp");
		model.addAttribute("pdf_word", ".pdf");
		return "ipp/ipp";
	}

```
-Controller example


#### Tests
```java
public class GetStudentTest {
	@Test
	public void testGetStudentUnknown() throws RemoteException{
		School school = Data.getSchoolGood();
		IPASIService201305 pasiProxy = new IPASIService201305Proxy(school.getEndpoint());
		StudentRequest request = new StudentRequest();
		CallerInfo callerInfo = Data.getCallerInfoGood();
		callerInfo.getUser().setOrganizationCode("A.9131");
		request.setCallerInfo(callerInfo);
		String[] stateProvinceIds = {"..."};
		EntityKeyVersionInfo entity = new EntityKeyVersionInfo();
		entity.setExpectedVersion(0l);
		entity.setKey("036452407");
		EntityKeyVersionInfo[] entities = {entity};
		request.setStateProvinceIds(entities);
		StudentResponse[] responses =  pasiProxy.getStudent(request, school);
		assertEquals(1, responses.length);
		StudentResponse studentResponse = responses[0];
		assertEquals("Unknown", studentResponse.getAvailabilityStatus());
		StudentInfo studentInfo = studentResponse.getStudent();
		CitizenshipStatusInfo[] citizenshipStatusInfo =  studentInfo.getCitizenshipStatuses();
		assertNull(citizenshipStatusInfo);
		ExtendedInfoList extendedPersonalInfo =  studentInfo.getExtendedPersonalInfo();
		StudentIdentificationInfo studentIdentificationInfo = studentInfo.getIdentificationRecord();
		Address[] otherAddresses =  studentInfo.getOtherAddresses();
		assertNull(otherAddresses);
		Name[] otherNames = studentInfo.getOtherNames();
		assertNull(otherNames);
		PhoneNumber[] otherPhoneNumbers = studentInfo.getOtherPhoneNumbers();
		assertNull(otherPhoneNumbers);
		Address preferredMailingAddress = studentInfo.getPreferredMailingAddress();
		assertNull(preferredMailingAddress);
		Name preferredName = studentInfo.getPreferredName();
		assertNull(preferredName);
		PhoneNumber preferredPhoneNumber = studentInfo.getPreferredPhoneNumber();
		assertNull(preferredPhoneNumber);
		assertNull(studentInfo.getPrimaryStateProvinceId());
		StudentDisclosureRestrictionInfo[] protectionSummary = studentInfo.getDisclosureRestrictions();
		assertNull(protectionSummary);
		String[] secondaryStateProvinceIds = studentInfo.getSecondaryStateProvinceIds();
		assertNull(secondaryStateProvinceIds);
		assertNull(studentInfo.getSection23Eligibility()); 
		assertEquals("999999999", studentInfo.getStateProvinceId());
		assertFalse(studentInfo.isIsDeactivated());
		assertFalse(studentInfo.isIsDeceased());
```
-used jUnit for testing

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = { "classpath:**/web.xml","/isis-data.xml","/isis-data-jdbc.xml",
"/isis-service.xml", "/isis-servlet.xml"})
public class PhoneNumberControllerIntegrationTest {
	 
	@Autowired
	private ApplicationContext applicationContext;

	private MockHttpServletRequest request;
	private MockHttpServletResponse response;
	private MockHttpSession session;
	private HandlerAdapter handlerAdapter;
	    
	@Autowired
	PhoneNumberController phoneController;

	@Before
	public void setUp() throws Exception {
		this.request = new MockHttpServletRequest();
	        this.response = new MockHttpServletResponse();
	        this.session = new MockHttpSession();

	        this.handlerAdapter = applicationContext.getBean(HandlerAdapter.class);
	}

	    ModelAndView handle(HttpServletRequest request, HttpServletResponse response)       throws Exception 
...
	@Test
	public void testFindNew() throws Exception {
	    	Integer scid = 61570;
	    	Integer rid = -1;
	    	String sid = "107309015";
	    	Integer pref = 0;
	    	Integer lid = -1;
	        request.setRequestURI("/phone/find/"+rid+"/"+scid+"/"+sid+"/"+pref+"/"+lid);
	        request.setMethod("GET");
	        final ModelAndView mav = handle(request, response);
	        assertViewName(mav, "pasi/managephone");
	        assertModelAttributeAvailable(mav, "today");
	        List<String> listables = new ArrayList<String>();
		listables.add("Yes");
		listables.add("No");
		listables.add("Unknown");
	        assertModelAttributeValue(mav, "listables", listables);
	        PhoneNumber phone = null;
	        assertNull(mav.getModel().get("phone"));
	        assertModelAttributeValue(mav, "sid", sid);
	        assertModelAttributeValue(mav, "scid", scid);
	        assertModelAttributeValue(mav, "preferred", pref);
	   }
		   
```
-mocks to test controllers

```java
public class AssignmentJdbcDaoTest extends DatabaseTestCase {
    
    private final static Log  log = LogFactory.getLog(AssignmentJdbcDaoTest.class);

    private ApplicationContext ctx;
    private String[] configLocations = { "isis-data-tests.xml",
            "isis-data-jdbc.xml","isis-service.xml" };
    AssignmentDao assignmentDao;
    protected void setUp() throws Exception {
        ctx = new ClassPathXmlApplicationContext(configLocations);
        assignmentDao= (AssignmentDao)ctx.getBean("assignmentDao");
        super.setUp();
    }

    protected IDatabaseConnection getConnection() throws Exception {
        DataSource ds = (DataSource) ctx.getBean("dataSource");
        return new DatabaseConnection(ds.getConnection());
    }

    protected IDataSet getDataSet() throws Exception {
        return new XmlDataSet(new FileInputStream("data/tests/c/_master.xml"));
    }
   
    	protected DatabaseOperation getSetUpOperation() throws Exception {
        	return DatabaseOperation.REFRESH;
	}
	public void testSaveAssignment()throws Exception{
    		Assignment assignment = new Assignment();
    		assignment.setCourseID(Data.COURSE_ID);
    		assignment.setCategoryID("...");
    		assignment.setAssignmentID(99999);
    		assignment.setGivenDate(DateUtil.getDate("10/10/2018"));
    		assignment.setDueDate(DateUtil.getDate("10/11/2018"));
    		assignment.setShortDesc("testdesc");
    		assignment.setFullDesc("testfulldesc");
    		assignment.setAssignValue(10);
    		assignment.setWeight(0);
    		assignment.setTermCode(Data.TERM);
    		assignment.setSchoolId(Data.SCHOOL_ID);
    		assignmentDao.saveAssignment(assignment);
    	
    		Assignment assignment2 = assignmentDao.getAssignment("99999", Data.TERM, Data.COURSE_ID,Data.SCHOOL_ID);
    		assertNotNull(assignment2);
    		assignmentDao.deleteAssignment(assignment);
    		assertNull(assignmentDao.getAssignment("99999", Data.TERM, Data.COURSE_ID,Data.SCHOOL_ID));
    	
    	}
```
-used dbUnit for data layer test, probably better to have used Spring tests 

#### frontend interfaces
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib uri="/WEB-INF/tlds/lists.tld" prefix="lst" %>
<c:url var="base" value="/" />
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Recognition Awards</title>
<link type="text/css" rel="stylesheet" href="https://ajax.googleapis.com/ajax/libs/jqueryui/1.9.2/themes/${existingTheme}/jquery-ui.css" />
<link rel="stylesheet" href="${base }css/demos.css" type="text/css" />
<link rel="stylesheet" href="${base }js/chosen/chosen.relative.css" />
<style type="text/css" title="currentStyle">
	@import "${base }css/demo_page.css";
	@import "${base }css/demo_table_jui.css";
	@import "${base }TableTools-2.0.1/media/css/TableTools_JUI.css";
</style>
<link rel="stylesheet" href="${base }css/process.css" type="text/css" />

</head> 
<body>
	<div id="light" class="white_content">Loading...<img src="${base }images/ajax-loader.gif" alt="loading" class="loading"><h2>${saying }</h2></div>
    	<div id="fade" class="black_overlay"></div> 

	<table class="header">
		<tr>
			<td class="offheader">
			<c:choose>
			<c:when test="${fn:startsWith(sessionScope.user.type,'d')==true}">
				<a class="head bootton"	href="${base }indexdirector.jsp?scid=${scid }">home</a>
			</c:when>
			<c:otherwise>
			<c:choose>
			<c:when test="${fn:startsWith(sessionScope.user.type,'a')==true}">
				<a class="head bootton"	href="${base }indexassist.jsp?scid=${scid}">home</a>
			</c:when>
			<c:otherwise>
				<a class="head bootton"	href="${base }index.jsp?scid=${scid }">home</a>
			</c:otherwise>
			</c:choose>
			</c:otherwise>
			</c:choose>
			</td>
		</tr>
	</table>
	
<div id="processing" class="ui-state-error ui-corner-all"></div>
	
<div class="ui-state-active ui-corner-all" style="margin-left: auto;margin-right: auto; width: 50%;">

<c:choose>
	<c:when test="${not empty hrcourses }">
	<c:forEach items="${hrcourses }" var="course">
		<c:choose>
			<c:when test="${course.id == courseId }">
				${course.formalName} (${course.teacherId })
			</c:when>
		<c:otherwise>
			<a class="bootton" href="${base }psi/recognition/award/${course.id}/-1/${scid}">${course.formalName} (${course.teacherId })</a>
		</c:otherwise>
		</c:choose>

	</c:forEach>
	</c:when>
	<c:otherwise>	
		Grades 	
 		<c:forEach items="${grades }" var="grade">
                	<c:choose>
			<c:when test="${gradePicked == grade.key}">
				 <b>${grade.value }</b>
			</c:when>
			<c:otherwise>
				<a class="bootton" href="${base }psi/recognition/award/-1/${grade.key}/${scid}">${grade.value } </a>
			</c:otherwise>
			</c:choose>
            	</c:forEach>
  <br/>
<c:forEach items="${courses }" var="course">
<c:choose>
	<c:when test="${course.id == courseId }">
		${course.formalName} (${course.teacherId })
	</c:when>
	<c:otherwise>
		<a class="bootton" href="${base }psi/recognition/award/${course.id}/${gradePicked}/${scid}">${course.formalName} (${course.teacherId })</a>
	</c:otherwise>
</c:choose>
</c:forEach>	
</c:otherwise>
</c:choose>

</div>
<br/>

 <c:if test="${not empty students }">
	 <select id="gt_reach">
 		<option value="1">greater than</option>
	 	<option value="2">less than</option>
 	</select>
  	REACH<input id="reach_limit"  />
  	<input type="hidden" name="scid" id="scid" value="${scid }" />
	<input type="hidden" name="cid" value="${courseId }" />
	<input type="hidden" name="y" value="${endyear }" />  
   	<fmt:formatDate pattern="MM/dd/yyyy" value="${now }" />
  
  	<table id="filled"  class="display">
 	<thead>
	<tr>
		<th>Name of Student</th><th>President's List</th><th>REACH Award</th><th>Personal Best</th>
		<th>Attendance <br/>Months Perfect</th><th>Attendance <br/>Days Absent for the year</th><th>Homework 100%</th><th>Homework</th>

		<c:forEach items="${options }" var="option">
			<th>${option.subjectName }</th>
		</c:forEach>

		<c:forEach items="${subjects }" var="subject">
			<th>AI - ${subject.subjectName }</th>
		</c:forEach>
		<c:forEach items="${subjects }" var="subject">
			<th>AE - ${subject.subjectName }</th>
		</c:forEach>
	</tr>
	</thead>

	<c:forEach items="${students }" var="student">
	<tr>
		<td >${student.givenName } ${student.surName }</td>
		<td><span id="president_${student.id }">${winners[student.id]['president'] }</span>
		<c:if test="${empty hrcourses }">
	 		<input class="bootton pres" type="button" id="yes_${student.id }" value="Yup" />
	 		<input class="bootton pres" type="button" id="no_${student.id }" value="Nope" />
	 	</c:if>
	 <br/>	  
		</td>
		<td>${reaches[student].amount }</td>
		<td>	</td>	
	<td>${attendances[student].amount }</td>
	<td>${attendanceDays[student.id] }</td>	
	<td>${homeworks[student].amount }</td>
	<td>	 
	</td>
	<c:forEach items="${options }" var="option">
		<td>${optionGrades[student][option.subjectId].finalgrade }</td>
	</c:forEach>	
	
	<c:forEach items="${subjects }" var="subject">
		<td>${improvements[student.id][subject.subjectId]}</td>
	</c:forEach>
	<c:forEach items="${subjects }" var="subject">
		<td>${excellents[student.id][subject.subjectId]}</td>
	</c:forEach>
	 
	</tr>
	</c:forEach>
</table>

</c:if>
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js"></script>
<script src="//ajax.googleapis.com/ajax/libs/jqueryui/1.9.2/jquery-ui.min.js"></script>
<script type="text/javascript" src="${base }js/confirm.js"></script>
<script type="text/javascript" charset="utf-8" src="${base }scripts/jquery.dataTables.js"></script>
<script type="text/javascript" charset="utf-8" src="${base }TableTools-2.0.1/media/js/ZeroClipboard.js"></script>
<script type="text/javascript" charset="utf-8" src="${base }TableTools-2.0.1/media/js/TableTools.js"></script>
<script>

$.fn.dataTableExt.afnFiltering.push(
	function( oSettings, aData, iDataIndex ) {
		var iMin = document.getElementById('reach_limit').value ;
		var gt = document.getElementById('gt_reach').value ;
		var iVersion = aData[2] == "" ? 0 : aData[2]*1;
		if( gt == 1){
				if ( iMin == "" )
		...
				return false;
			}
		
		}
	);


$(function() {
	var $loading = $('<img src="${base }images/ajax-loader.gif" alt="loading" class="loading">');
			 
	$( ".bootton" ).button();
		
	setTimeout(function(){$('#light').hide();
		$('#fade').hide();},3000);
		
	var asInitVals = new Array();
	var oTable = $('#filled').dataTable( {
		"bJQueryUI": true,
		"bPaginate": false,
		"sDom": '<"H"Tfrlp>t<"F"ip>',
		"oTableTools": {
				"aButtons": [
		{
			"sExtends": "csv",
    			"sButtonText": "Download",
    			"mColumns": "visible",
   			 "sFieldBoundary": ''  
    			}
	...
			},
			 "sScrollX": "100%"
			
		} );
	$("div.toolbar").html('');  
		
	$('#reach_limit').keyup( function() { oTable.fnDraw(); } );
	$('#homework_limit').keyup( function() { oTable.fnDraw(); } );
	$( "#gt_reach" ).change(function() {
		oTable.fnDraw();
		});
	    
     $(".pres").click(function() {
    	var idd = $(this).attr('id');
    	var answ = idd.substring(0, idd.indexOf('_'));
	idd = idd.substring(idd.indexOf('_'),idd.length);
	$.post("${base}psi/recognition/savePresident", 
        	{pick: $(this).attr('id'),
        	  scid: $('#scid').val()}, 
        	function(data){        				  
        		if(data != ''){
        			var tar = $('#processing');
        			 tar.html(data);
        			 tar.fadeIn(2000).animate({duration: 'slow', queue: false}, function() {
        			    });
        			 tar.fadeOut(2000);
        			 $('#president'+idd).html(answ);        				   
        		}
        	});
    	 
     	});
	});	
	</script>

</body>
</html>
```
-used jquery to makes calls to Spring controllers to dynamically get data

```html
<%@ page language="java" 
	import="java.util.*,domain.*,dao.*,dao.jdbc.*,util.AppCon" errorPage="/error.jsp"%>
<!DOCTYPE html>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<html>
<head>
<script>

$(function() {
	
	jQuery.validator.addMethod("dateComparison",function(value,element) {
	 	var result= true;			
	    	if($("#expiry_phone").val().length > 0 )    	{
	        	var dateArray= $("#effective_phone").val().split("/");
	        	var startDateObj= new Date(dateArray[0],(dateArray[1]-1),dateArray[2],0,0,0,0);	     
	    		var endDateArray= $("#expiry_phone").val().split("/");
	    		var endDateObj= new Date(endDateArray[0],(endDateArray[1]-1),endDateArray[2],0,0,0,0);
	    		var startDateMilliseconds= startDateObj.getTime();
	    		var endDateMilliseconds= endDateObj.getTime();
					
	    		if (endDateMilliseconds <= startDateMilliseconds)   {
	        		result= false;
	      		}
	    
	      	} 
	    return result;
	 },"Invalid Expiry Date. The Phone Number Expiry Date provided must be later than Effective Date.");
	
	$( ".bootton" ).button();
	 var validate =	$("#phoneForm").validate({
	    	rules: {
	    	number: {
			required: true,
	    		minlength: 10,
			maxlength: 10,
			digits: true
			},
		ext: {
			digits: true
			},
		expiry: {
			dateComparison: true
			},
		effective_phone: {
			required: true
		}
	      },
	      messages: {
	    	  number: {
	    		required: "Phone Number is Required",
	              	minlength: "Invalid Phone Number.  The phone number provided must be 10 characters long when starting with a number 2 through 9. ",
	              	maxlength: "Invalid Phone Number. The phone number provided must be 10 characters long when starting with a number 2 through 9.",
	              	digits: "Invalid Phone Number.  The phone number provided must only contain numbers (0 through 9) or +."
	        	},
	        ext: {
	              digits: "Invalid Extension. The Extension provided  must only be numbers (0 through 9)."
	        },
		 expiry: {
		       dateComparison: "Invalid Expiry Date. The Phone Number Expiry Date provided must be later than Effective Date."
		 	    },
		 effective_phone: {
		       	required: "Effective Date is Required"
		  }
	      },
	      errorLabelContainer: $("div#error_phone"),
	      onkeyup: false
	      
	    });
	 $( ".datepicker_phone" ).datepicker({
			changeMonth: true,
			changeYear: true,
			dateFormat: 'yy/mm/dd'
		});
});

</script>

</head>
<body>
	<c:set var="disableexpire" value="true"></c:set>
	<c:forEach items="${sessionScope.studentCombined.studentInfo.otherPhoneNumbers }" var="chph">
		<c:if test="${empty chph.expiryDate.time && chph.refId != rid}">
			<c:set var="disableexpire" value="false"></c:set>
		</c:if>
	</c:forEach>
	<c:if test="${empty sessionScope.studentCombined.studentInfo.preferredPhoneNumber.expiryDate.time && 									sessionScope.studentCombined.studentInfo.preferredPhoneNumber.refId != rid}">
		<c:set var="disableexpire" value="false"></c:set>
	</c:if>

	<div id="topLevel">
		<div class="ui-widget-content ui-corner-top container_no error" id="error_phone" style="display: none;">
			<h3 class="ui-dialog-titlebar ui-widget-header ui-corner-top">Validation Summary</h3>
	</div>
	<br />

	<form method="post" id="phoneForm">
		<input type="hidden" name="scid" value="${scid }" />
		<c:if test="${not empty phone.id }">
			<input type="hidden" name="lid" value="${phone.id }" />
		</c:if>
		<div class="ui-widget-content ui-corner-top container_no">
		<c:if test="${not empty rid }">
			Reference# ${rid }
			<input type='hidden' name='rid' value="${rid }" />
		</c:if>
		<input type='hidden' name='sid' value="${sid }" />
		<div class="form_last_row">
			<div class="form_field">
				<label>Phone Number </label> <input type="text" name='number' id="number" value="${phone.number }" />
				<div class="required">*</div>
			</div>
		</div>
		<div class="form_last_row">
			<div class="form_field">
				<label>Extension </label> <input type="text" name='ext' id="ext" value="${phone.extension }" />
			</div>
		</div>

...

		<div class="form_last_row">
			<div class="form_field">
				<label>Expire all active phone records </label>
				<c:choose>
					<c:when test="${disableexpire }">
						<select  name="expireactive">
							<option value="N" selected="selected">No</option>
						</select>
					</c:when>
				<c:otherwise>
					<select name="expireactive">
					<c:choose>
						<c:when test="${not empty rid }">
						<option value="N" selected="selected">No</option>
						<option value="Y">Yes</option>
					</c:when>
					<c:otherwise>
						<option value="N">No</option>
						<option value="Y" selected="selected">Yes</option>
					</c:otherwise>
				</c:choose>
				</select>
				</c:otherwise>
						</c:choose>
					</div>
				</div>

			</div>
		</form>
	</div>
	<div id="loading"></div>

</body>
</html>

```

#### Misc

```java
public aspect AttendanceAspect {

	private HttpServletRequest request;
	
	pointcut  logSaveAttendance2() :  execution(* servlets.Attendance*Servlet.doPost*(..));
	
	before() : logSaveAttendance2() {
		Object[] vs = thisJoinPoint.getArgs();
		request = (HttpServletRequest) vs[0];
	}
	
	pointcut  logSaveAttendance() :  execution(* dao.AttendanceDao.saveAttendance*(..));
	
	before() : logSaveAttendance() {
		UpdatesDao updatesDao = AppCon.getUpdatesDao();
		StudentDao studentDao = AppCon.getStudentDao();
...
```
-Used aspect oriented code for certain cross cutting concerns like auditing/logging certain important data.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns:p="http://www.springframework.org/schema/p"
 xmlns:task="http://www.springframework.org/schema/task"
 xsi:schemaLocation="http://www.springframework.org/schema/beans 
 http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
 http://www.springframework.org/schema/task
 http://www.springframework.org/schema/task/spring-task-3.0.xsd">
 
 <bean id="weeklyGradeEmailJob"
      class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
    <property name="targetObject" ref="reportService"/>
    <property name="targetMethod" value="sendWeeklyGradesEmail" />
</bean>
<bean id="cronEmailTrigger"
      class="org.springframework.scheduling.quartz.CronTriggerBean">
    <property name="jobDetail" ref="weeklyGradeEmailJob"/>
    <property name="cronExpression" value="0 59 23 ? * SUN" />
  </bean>
```
-Used Quartz Scheduling for a variety of tasks, like weekly reports, etc

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">
    
    <bean id="reachDaoHibernate" class="dao.hibernate.ReachDaoImpl">
		<property name="sessionFactory" ref="sessionFactory" />
	</bean>
    
    <bean id="examDao" class="dao.exam.ExamJdbcDaoImpl">
		<property name="dataSource" ref="dataSource" />
	</bean>
 ``` 
 -Started off using xml files for Spring configuration, was in the process of updating to just using Java classes and defining beans using annotations
 
```java
 
click.expand=Click to Expand
click.collapse=Click to Collapse
find.student=Look up Student
find.student.desc=Find out what other courses your students are enrolled, their marks, attendances, last years report card and if you need any other info reported let the appropriate people know.
image.question=images/question.gif
granted.access=Granted Access:

```
-used xxx_en.properties files for many front end, there was talk with one school about translating to spanish as well
  

## About Me <a name="about"></a>

####  Just to give a little insight into my personality, here was the message Calgary Academy wrote on their weekly newsletter about my departure.
[Click here to open pdf](https://github.com/jaysait/CA-Student-Info-System/blob/master/_CAConnectionShowingDepartingMessage.pdf)

#### Because I loaded everything upfront in certain areas of the program, to pass the initial wait I would include random made up facts (late show style) about individual program plans (IPP)/related topics to help pass the time, also shows a little bit about my sense of humour or lack there of
- One out of every 15 IPPs involve making better jerky
- In a small tribe in the Amazon, it's believed that IPPs are the work of sorcerers
- Raccoons are the only other documented animal that also practise IPPs...and as you'd expect most of the goals pertain to Algebra or opening cans
- the world's longest IPP measured 954 pages long...the irony...most were goals of getting to the point quicker
- In France IPPs are called PPIs...huh
- Did you know the first recorded IPP was in the Mesolithic age on rock...it said long term goal = do not get eaten by dinosaur...sadly without properly written objectives he was eaten
- Did you know the phrase IPP was an accident...he just had a stutter and had to go to the washroom...sadly he didn't get help for either
- 20% of IPPs from the 80s contain mustard stains on them
- During the war when paper/lead was sparse, IPPs were written on barns with honey
- The most common word used in IPPs...the, the second most common word...Cockamamie
- Countries that don't practise IPPs are 80% more likely to not know what a wheel is for but 20% more likely to own a cotton candy machine
- People from Romania, pronounce them 'YIPs' and people from Saskatchewan pronounce them 'dirty IPPys'
- 1 out of 4 teachers said that 25% of IPPs they write involve 'student X needs to stop sticking Y in Z so much'...sadly the success rate on those is only 30%
- IPPs are the 2nd most contributed tool of success for kids...the first one...tang or is it love
- The kid with a IPP with the record most goals achieved of 78 in 1 year has since had his record stripped due to the use of Human Growth Hormone.
- Help I'm stuck in a cupboard
- Calgary Academy staff are known worldwide for their great IPPs...that and consuming almost 2x the amount of alcohol of the average sailor
- 1 out of 3 teachers will lie on their IPPs to get a bigger bonus...so look to your left and right...one of you...shame
- Most people forget to do the M in SMART goals, that and their left sock...coincidence??
- Today only 2 for 1 deal on Huckleberry Pecan Marmalade, and tell them Lily sent you for a free melon baller
- Little known fact, the movie Titanic was based on a 7th grader's IPP in Delaware...or was it Weekend at Bernies...ahhh wikipedia...so factual
- If you put all the ipps in a row, it would go around the world 573 times...on a unrelated note...the rainforest is down  5%
- IPP goals come true 45% more often when a child puts it under their pillow and wishes it to come true or is it hard work/dedication, nope lets go with wishes
- If you're here working on ipps...WHOS WATCHING THE KIDS!!
- If any of your IPP goals involve 'pees' or 'poops' less...maybe IPPs shouldn't be a priority
- Have you been told lately you make such a difference, even if you don't always see it and look at you...did someone say teacher or runway model
- Without someone like you there wouldn't be someone like me...makes you kind of question your whole career path don't it 
- When was the last time you wrote an IPP for yourself...give it a try, it'll make you feel like a kid again. 
- If bored create unattainable goals for the trouble kids, like 'Timmy should persue being a doctor, he really is good at it' even though he gets like 50%...then sit back and wait for the fun
- If your are having a difficult time coming up with IPPs, come to the back, there'll be a white van...bring $20 (unmarked bills but preferably twoonies) and knock in the theme of the tv show 'Magnum PI'"


