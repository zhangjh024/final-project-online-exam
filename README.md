


####  作业说明

这个项目的框架是别人做的，我也只是拿来学习，虽然如此，我也从中得到了一些收获，对以后的项目开发有了一些更深入的认识，
经过本次课程的学习，我也逐渐掌握了git的使用方法，这是一个很方便的合作开发工具，也利于共享和互相学习。对于真正的项目实战来说，
我需要走的路还更长，但这门课让我对这个有了一个入门的认识，对于项目从害怕恐惧逃避，到逐渐地面对，解决，完成它。


本次项目学习：

1.在线考试系统主要包括chart、handler、dao、po、service、interceptor、util包，其中chart类包主要是实现班级的学生总量，相关图表，Json数据的生成；handler实现前后台数据交换；dao包存放dao接口，用于存放系统与数据库进行数据交互的接口；；service包存放逻辑控制接口，用于控制系统业务逻辑，util包存放工具类，用于实现类型转换。

2.登陆拦截器与学生登陆
//  public class LoginInterceptor extends HandlerInterceptorAdapter {

	private Logger logger = Logger.getLogger(LoginInterceptor.class);
	
	@Override
	public boolean preHandle(HttpServletRequest request,
			HttpServletResponse response, Object handler) throws Exception {
		
		HttpSession session = request.getSession();
		if (session.getAttribute("loginTeacher") != null) {
			return true;
		} else {
			logger.info("检测到未登录访问后台内容操作");
			//如果没有登录，跳转至登录页面
			response.sendRedirect("admin/login.jsp");
			
			return false;
		}
	}
}//
/**
	 * 学生考试登录验证
	 * 
	 * 此处验证并不合理 登录验证实现如下：
	 *   前台学生登录传入账户，后台根据账户获取学生密码
	 *   返回学生密码，前台登录焦点离开密码框使用 JavaScript 判断
	 * 
	 * @param studentAccount 学生登录账户
	 * @param response
	 * @throws IOException
	 */
	@RequestMapping("/validateLoginStudent")
	public void validateLoginStudent(@RequestParam("studentAccount") String studentAccount,
			HttpServletResponse response) throws IOException {
		logger.info("学生账户 "+studentAccount+"，尝试登录考试");
		
		//获取需要登录的学生对象
		StudentInfo student = studentInfoService.getStudentByAccountAndPwd(studentAccount);
		
		if (student == null) {
			logger.error("登录学生账户 "+studentAccount+" 不存在");
			response.getWriter().print("n");
		} else {
			logger.error("登录学生账户 "+studentAccount+" 存在");
			response.getWriter().print(student.getStudentPwd());
		}
	}
	
	/**
	 * 学生登录考试
	 * @param student 登录学生
	 * @param request
	 * @return
	 */
	@RequestMapping(value="/studentLogin", method=RequestMethod.POST)
	public ModelAndView studentLogin(StudentInfo student, HttpServletRequest request) {
		ModelAndView model = new ModelAndView();
		StudentInfo loginStudent = studentInfoService.getStudentByAccountAndPwd(student.getStudentAccount());
		logger.info("学生 "+loginStudent+" 有效登录");
		
		request.getSession().setAttribute("loginStudent", loginStudent);
		
		model.setViewName("reception/suc");
		model.addObject("success", "登录成功");
		
		return model;
	}
  



3.试题的增删改查


/**
	 * 添加试题
	 * @param subject
	 * @param response
	 * @throws IOException
	 */
	@RequestMapping(value="/addSubject", method=RequestMethod.POST)
	public void addSubject(SubjectInfo subject, HttpServletResponse response) throws IOException {
		int row = subjectInfoService.isAddSubject(subject);
		
		response.getWriter().print("试题添加成功!");
	}
	
	
	/**
	 * 删除试题
	 * @param subjectId
	 * @param response
	 * @throws IOException
	 */
	@RequestMapping(value="/delSubject", method=RequestMethod.POST)
	public void delSubject(@RequestParam("subjectId") Integer subjectId,
			HttpServletResponse response) throws IOException {
		logger.info("删除试题 "+subjectId);
		
		int row = subjectInfoService.isDelSubject(subjectId);
		
		if (row > 0) {
			response.getWriter().print("t");
		} else {
			response.getWriter().print("f");			
		}
	}
	
	
	/**
	 * 修改试题 -- 获取待修改试题信息
	 * @param subjectId
	 * @return
	 */
	@RequestMapping("/subject/{subjectId}")
	public ModelAndView updateSubject(@PathVariable("subjectId") Integer subjectId) {
		logger.info("修改试题 "+subjectId+" 的信息(获取试题信息)");
		
		SubjectInfo subject = subjectInfoService.getSubjectWithId(subjectId);
		
		ModelAndView model = new ModelAndView("/admin/subject-test");
		model.addObject("subject", subject);
		
		return model;
	}
	
	/**
	 * 修改试题
	 * @param subject
	 * @param response
	 * @throws IOException
	 */
	@RequestMapping(value="/updateSubject", method=RequestMethod.POST)
	public void updateSubject(SubjectInfo subject, HttpServletResponse response) throws IOException {
		logger.info("修改试题 "+subject.getSubjectId()+" 的信息(正式)");

		int row = subjectInfoService.isUpdateSubject(subject);
		if (row > 0) {
			response.getWriter().print("试题修改成功!");
		} else {
			response.getWriter().print("试题修改失败!");
		}
	}
//
4.
#### 

