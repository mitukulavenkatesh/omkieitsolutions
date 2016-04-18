# omkieitsolutions 
repository another

<?php

defined('BASEPATH') OR exit('No direct script access allowed');

class Institute extends CI_Controller {

    function __construct() {
        parent::__construct();

        $this->load->model('Institute_model');
        $this->load->model('Model_ope');
        $this->load->library("pagination");
        $this->load->model('Admin_panel');
    }

    public function edit_institute() {
        $this->load->model('Institute_model');
        $this->load->view('templates/head');
      //  $data ['videos'] = $this->Institute_model->get_videos();
        $this->load->view('templates/header');
        $this->load->view('institute/edit');
    }

    public function change_password_institute() {
           if (!empty($_SESSION['logged_in'])) {
        $data['institute'] = $this->Institute_model->get_institute_info();
        $this->load->view('templates/head');
        $this->load->view('templates/header');
        $this->load->view('institute/change_password', $data);
    }
    else{
        redirect();
    }
           }

    public function institute_subscribers() {
           if (!empty($_SESSION['logged_in'])) {
        $this->pagination->initialize($this->set_pagination_subs());
        $page_sub = ($this->uri->segment(2)) ? $this->uri->segment(2) : 0;
        $data["subscribed"] = $this->Institute_model->get_subscribed_list($this->set_pagination_subs(), $page_sub);
        $data["pagess"] = $this->pagination->create_links();
        $data['institute'] = $this->Institute_model->get_institute_info();
        //$data['subscribed'] = $this->Institute_model->get_subscribed_list();
        $data['subscribers'] = $this->Institute_model->get_subscribers();
        $data['subscribe'] = $this->Institute_model->get_timings();
        $this->load->view('templates/head');
        $this->load->view('templates/header');
        $this->load->view('institute/subscribers', $data);
    }
    else{
        redirect();
    }
           }

    public function add_professors_institute() {
          if (!empty($_SESSION['logged_in'])) {
        $data['institute'] = $this->Institute_model->get_institute_info();
        $this->pagination->initialize($this->set_pagination());
        $page = ($this->uri->segment(3)) ? $this->uri->segment(3) : 0;
        $data["professors"] = $this->Institute_model->get_professors($this->set_pagination(), $page);
        $data["links"] = $this->pagination->create_links();
        $data['professor'] = $this->Institute_model->getone_professor();
        $this->load->view('templates/head');
        $this->load->view('templates/header');
        $this->load->view('institute/add_professors', $data);
    }
    else{
        redirect();
    }
          }

    public function add_courses_institute() {
          if (!empty($_SESSION['logged_in'])) {
        $data['institute'] = $this->Institute_model->get_institute_info();
        $data['courses'] = $this->Institute_model->get_course();
        $data['faculty'] = $this->Institute_model->get_faculty();
        $this->pagination->initialize($this->set_pagination_courses());
        $page_ = ($this->uri->segment(3)) ? $this->uri->segment(3) : 0;
        //$data['results'] = $this->Institute_model->get_courses_pagen($this->set_pagination_courses(), $page_);
        $this->load->view('templates/head');
        $this->load->view('templates/header');
        $this->load->view('institute/add_courses', $data);
    }
    else{
        redirect();
    }
          }

//    public function institute_inbox() {
//        $data['institute'] = $this->Institute_model->get_institute_info();
//        $this->load->view('templates/head');
//        $this->load->view('templates/header');
//        $this->load->view('institute/inbox', $data);
//    }

    public function load_account_page() {
  if (!empty($_SESSION['logged_in'])) {
        //The below page should'nt be shown when user not logged in
        
        //$data['images'] = $this->Institute_model->get_images();

        $data['institute'] = $this->Institute_model->get_institute_info();
        $this->load->view('templates/head');
        $this->load->view('templates/header');
        $this->load->view('institute/edit', $data);
    }
    else{
        redirect();
    }
  }

    public function redi() {
        redirect('Institute/load_account_page#edit_institute');
    }

    public function redi_prof() {
        $this->pagination->initialize($this->set_pagination());
        $page = ($this->uri->segment(3)) ? $this->uri->segment(3) : 0;
        $data["professors"] = $this->Institute_model->get_professors($this->set_pagination(), $page);
        $data["links"] = $this->pagination->create_links();
        redirect(site_url('Institute/load_account_page#add_professor'), $data, 'refresh');
    }

    public function get_inst_details() {
        $this->load->model('Institute_model');
        return $this->Institute_model->get_inst_details_m();
    }

    public function set_pagination() {


        $config = array();
        $config["base_url"] = base_url() . "index.php/Institute/add_professors";
        $config["total_rows"] = $this->Institute_model->record_count();
        $config["per_page"] = 5;
        $config["uri_segment"] = 3;

        $this->pagination->initialize($config);

        $page = ($this->uri->segment(3)) ? $this->uri->segment(3) : 0;
        return $config;
    }

    public function set_pagination_courses() {


        $config = array();
        $config["base_url"] = base_url() . "index.php/Institute/add_courses_institute";
        $config["total_rows"] = $this->Institute_model->record_count_course();
        $config["per_page"] = 5;
        $config["uri_segment"] = 2;

        $this->pagination->initialize($config);

        $page_ = ($this->uri->segment(2)) ? $this->uri->segment(2) : 0;
        return $config;
    }

    public function set_pagination_() {


        $config = array();
        $config["base_url"] = base_url() . "index.php/Institute/institute_profile_view";
        $config["total_rows"] = $this->Institute_model->record_count_course();
        $config["per_page"] = 5;
        $config["uri_segment"] = 2;

        $this->pagination->initialize($config);

        $page_ = ($this->uri->segment(2)) ? $this->uri->segment(2) : 0;
        return $config;
    }

    public function set_pagination_subs() {


        $config = array();
        $config["base_url"] = base_url() . "index.php/Institute/institute_profile_view";
        $config["total_rows"] = $this->Institute_model->record_count_subscribers();
        $config["per_page"] = 5;
        $config["uri_segment"] = 2;

        $this->pagination->initialize($config);

        $page_ = ($this->uri->segment(2)) ? $this->uri->segment(2) : 0;
        return $config;
    }

    public function updateinfo() {
        $this->load->model('Institute_model');
        //$this->Data_model->updateinfo_m();
        //$this->do_upload();

        $config['upload_path'] = './uploads/institute/inst_display_pics/';
        $config['allowed_types'] = 'gif|jpg|png|jpeg';
        $this->load->library('upload', $config);
        $this->upload->initialize($config);
        $this->upload->do_upload();

        $data = $this->upload->data();

        if ($this->upload->do_upload()) {


            //echo 'uploads/institute/display_pics/'.$data['raw_name'].$data['file_ext'];

            $dat = array(
                'inst_name' => $this->input->post('inst_name'),
                'inst_contact' => $this->input->post('contact'),
                'address' => $this->input->post('address'),
                'about_us' => $this->input->post('about'),
                'logo_path' => 'uploads/institute/inst_display_pics/' . $data['raw_name'] . $data['file_ext'],
                //'logo_path' => $this->input->post('image'),
                'inst_email' => $this->input->post('email'),
                'longitude' => $this->input->post('longitude'),
                'lattitude' => $this->input->post('lattitude')
            );

            $this->db->where('inst_id', $_SESSION['id']);
            $this->db->update('institutes', $dat);
            //$this->load->view('templates/student_head');
            //$this->load->view('account/inst_account_page',$data);
            $this->edit_institute();
        } else {
            //echo $this->upload->display_errors();

            $dat2 = array(
                'inst_name' => $this->input->post('inst_name'),
                'inst_contact' => $this->input->post('contact'),
                'address' => $this->input->post('address'),
                'about_us' => $this->input->post('about'),
                //'logo_path' => $this->input->post('image'),
                'inst_email' => $this->input->post('email'),
                'longitude' => $this->input->post('longitude'),
                'lattitude' => $this->input->post('lattitude')
            );

            $this->db->where('inst_id', $_SESSION['id']);
            $this->db->update('institutes', $dat2);
            $data['institute'] = $this->Institute_model->get_institute_info();
            //$this->load->view('templates/student_head');
            $data['update'] = 'Updated Successfully..!';
            $this->load->view('templates/head');
            $this->load->view('templates/header');
            $this->load->view('institute/edit', $data);
            //$this->redi();
        }
    }

    //images upload to gallery

    function do_upload() {

        $this->load->model('Institute_model');

        $this->load->library('upload');

        $files = $_FILES;
        $cpt = count($_FILES['images']['name']);
        for ($i = 0; $i < $cpt; $i++) {
            $_FILES['images']['name'] = $files['images']['name'][$i];
            $_FILES['images']['type'] = $files['images']['type'][$i];
            $_FILES['images']['tmp_name'] = $files['images']['tmp_name'][$i];
            $_FILES['images']['error'] = $files['images']['error'][$i];
            $_FILES['images']['size'] = $files['images']['size'][$i];

            $this->upload->initialize($this->set_upload_options());
            $this->upload->do_upload('images');
            if ($this->upload->do_upload() == FALSE) {


                $data['images'] = $this->Institute_model->get_images();

                $data['institute'] = $this->Institute_model->get_institute_info();
                //$data['err'] = 'Error while Uploading..';
                $data['succes'] = 'Photos Uploaded Successfully';
                $this->load->view('templates/head');
                $this->load->view('templates/header');
                $this->load->view('institute/edit', $data);
            } else {
                
            }

            $data = $this->upload->data();
            //echo $data;
            $file = array(
                'img_name' => $data['raw_name'],
                'thumb_name' => $data['raw_name'] . '_thumb',
                'ext' => $data['file_ext']
            );

            $data = array(
                'inst_id' => $_SESSION['id'],
                'images' => 'uploads/institute/gallery/' . $file['img_name'] . $file['ext']
            );
            $this->Institute_model->add_image($data);


            $data['institute'] = $this->Institute_model->get_institute_info();
            $data['images'] = $this->Institute_model->get_images();

            $data['succes'] = 'Photos Uploaded Successfully';
            $this->load->view('templates/head');
            $this->load->view('templates/header');
            $this->load->view('institute/edit', $data);
            // }
        }
    }

    private function set_upload_options() {
        //upload an image options
        $config = array();
        $config['upload_path'] = './uploads/institute/gallery/';
        $config['allowed_types'] = 'gif|jpg|png|jpeg';
        $config['max_size'] = '5120';
        $config['overwrite'] = FALSE;

        return $config;
    }

    public function change_password() {

        $this->form_validation->set_rules('old_pwd', 'Old Password', 'trim|required');
        $this->form_validation->set_rules('new_pwd', 'New Password', 'trim|required');
        $this->form_validation->set_rules('conf_new_pwd', 'Confirm Password', 'trim|required|matches[new_pwd]');

        if ($this->form_validation->run() == FALSE) {//if returned False
            $data['institute'] = $this->Institute_model->get_institute_info();

            //$data['error'] = ' Please Enter correct password.';
            $this->load->view('templates/head');
            $this->load->view('templates/header');
            $this->load->view('institute/change_password', $data);
            // $this->redi();
        } else {//if true
            if (md5($this->input->post('old_pwd')) != $this->input->post('old_pwd_en')) {
                $this->load->view('templates/head');
                $data['error'] = 'Incorrect Old Password. Please Enter correct password.';


                $data['institute'] = $this->Institute_model->get_institute_info();

                //$this->redi($data);
                $this->load->view('templates/header');
                $this->load->view('institute/change_password', $data);
            } else {
                $this->Institute_model->change_password_i();
                $data['institute'] = $this->Institute_model->get_institute_info();

                $data['message'] = 'Password Changed Successfully';
                $this->load->view('templates/head');
                $this->load->view('templates/header');
                $this->load->view('institute/change_password', $data);
            }
        }
    }

    public function add_professors() {

        $this->form_validation->set_rules('name', 'Professor Name', 'trim|required');
        $this->form_validation->set_rules('email', 'Email', 'trim|required');
        //$this->form_validation->set_rules('mob', 'Mobile No', 'trim|required');
        $this->form_validation->set_rules('co_sub', 'Course and Subject', 'trim|required');

        if ($this->form_validation->run() == FALSE) {
            $this->form_validation->set_message('required', 'All Fields Are Required');

            $this->pagination->initialize($this->set_pagination());
            $page = ($this->uri->segment(3)) ? $this->uri->segment(3) : 0;
            $data["professors"] = $this->Institute_model->get_professors($this->set_pagination(), $page);
            $data["links"] = $this->pagination->create_links();
            //$data['institute'] = $this->Institute_model->get_inst_details_m();
            $data['institute'] = $this->Institute_model->get_institute_info();
            $data['professor'] = $this->Institute_model->getone_professor();
            $this->load->view('templates/head');
            $this->load->view('templates/header');
            $this->load->view('institute/add_professors', $data);
            //redirect('Institute/load_account_page#edit_institute');
        } else {
            $this->load->model('Institute_model');
            $config['upload_path'] = './uploads/institute/professor_pics/';
            $config['allowed_types'] = 'gif|jpg|png|jpeg';
            $this->load->library('upload', $config);
            $this->upload->initialize($config);
            $this->upload->do_upload('photo');
            $data = $this->upload->data();

            $file = array(
                'img_name' => $data['raw_name'],
                'thumb_name' => $data['raw_name'] . '_thumb',
                'ext' => $data['file_ext']
            );

            $data = array(
                'inst_id' => $_SESSION['id'],
                'prof_name' => $this->input->post('name'),
                'email' => $this->input->post('email'),
                'mobile' => $this->input->post('mob'),
                'phone_no' => $this->input->post('phone'),
                'courses' => $this->input->post('co_sub'),
                'worked_at' => $this->input->post('working'),
                'position' => $this->input->post('position'),
                'achievements' => $this->input->post('seminars'),
                'experience' => $this->input->post('exp'),
                'description' => $this->input->post('des'),
                'prof_pic' => 'uploads/institute/professor_pics/' . $file['img_name'] . $file['ext'],
            );

            $this->Institute_model->add_prof($data);
            $this->form_validation->set_message('success', 'Professor Added Successfully');
            $data['message'] = 'Professor Added Successfully';

            $this->pagination->initialize($this->set_pagination());
            $page = ($this->uri->segment(3)) ? $this->uri->segment(3) : 0;
            $data["professors"] = $this->Institute_model->get_professors($this->set_pagination(), $page);
            $data["links"] = $this->pagination->create_links();
            //$data['institute'] = $this->Institute_model->get_inst_details_m();
            $data['institute'] = $this->Institute_model->get_institute_info();
            $data['professor'] = $this->Institute_model->getone_professor();
            $this->load->view('templates/head');
            $this->load->view('templates/header');
            $this->load->view('institute/add_professors', $data);
        }
    }

    public function update_professor() {

        $this->load->model('Institute_model');
        $config['upload_path'] = './uploads/institute/professor_pics/';
        $config['allowed_types'] = 'gif|jpg|png|jpeg';
        $this->load->library('upload', $config);
        $this->upload->initialize($config);
        $this->upload->do_upload();
        $data = $this->upload->data();

        $file = array(
            'img_name' => $data['raw_name'],
            'thumb_name' => $data['raw_name'] . '_thumb',
            'ext' => $data['file_ext']
        );
        if ($this->upload->do_upload()) {

            $id = $this->input->post('profid_');
            $data = array(
                'inst_id' => $_SESSION['id'],
                'prof_name' => $this->input->post('update-profname'),
                'email' => $this->input->post('update-email'),
                'mobile' => $this->input->post('update-mob'),
                'phone_no' => $this->input->post('update-phone'),
                'courses' => $this->input->post('update-sub'),
                'worked_at' => $this->input->post('update-working'),
                'position' => $this->input->post('update-position'),
                'achievements' => $this->input->post('update-achive'),
                'experience' => $this->input->post('update-exp'),
                'description' => $this->input->post('update-des'),
                'prof_pic' => 'uploads/institute/professor_pics/' . $file['img_name'] . $file['ext'],
            );

            $this->db->where('prof_id', $id);
            $this->db->update('professors', $data);
            //$this->load->view('templates/header');
            //$this->load->view('account/inst_account_page',$data);
            $this->add_professors_institute();
        } else {

            $id = $this->input->post('profid_');
            $data = array(
                'inst_id' => $_SESSION['id'],
                'prof_name' => $this->input->post('update-profname'),
                'email' => $this->input->post('update-email'),
                'mobile' => $this->input->post('update-mob'),
                'phone_no' => $this->input->post('update-phone'),
                'courses' => $this->input->post('update-sub'),
                'worked_at' => $this->input->post('update-working'),
                'position' => $this->input->post('update-position'),
                'achievements' => $this->input->post('update-achive'),
                'experience' => $this->input->post('update-exp'),
                'description' => $this->input->post('update-des')
                    //'prof_pic'    =>'uploads/institute/profile_pics/'.$file['img_name'].$file['ext'],
            );

            $this->db->where('prof_id', $id);
            $this->db->update('professors', $data);
            //$this->load->view('templates/header');
            //$this->load->view('account/inst_account_page',$data);
            $this->add_professors_institute();
        }
    }

    public function professor_profile($id) {

        $this->load->model('Institute_model');
        $data['id'] = $id;
        $data['prof'] = $this->Institute_model->getprof_view($id);

        $this->pagination->initialize($this->set_pagination());
        $page = ($this->uri->segment(3)) ? $this->uri->segment(3) : 0;
        $data["professors"] = $this->Institute_model->get_professors($this->set_pagination(), $page);
        $data["links"] = $this->pagination->create_links();
        //$data['images'] = $this->Institute_model->get_images();
        $this->pagination->initialize($this->set_pagination_subs());
        $page_sub = ($this->uri->segment(2)) ? $this->uri->segment(2) : 0;
        $data["subscribed"] = $this->Institute_model->get_subscribed_list($this->set_pagination_subs(), $page_sub);
        $data["pagess"] = $this->pagination->create_links();
        //$data['subscribed'] = $this->Institute_model->get_subscribed_list();
        $data['subscribe'] = $this->Institute_model->get_timings();
        $data['subscribers'] = $this->Institute_model->get_subscribers();
        $this->pagination->initialize($this->set_pagination_courses());
        $page_ = ($this->uri->segment(2)) ? $this->uri->segment(2) : 0;
        //$data["results"] = $this->Institute_model->get_courses_pagen($this->set_pagination_courses(), $page_);
        //$data['counts'] = $this->Institute_model->count();
        //$data['professors'] = $this->Institute_model->get_professors();  
        $this->load->view('institute/professor_profile', $data);
    }

    public function professor_delete($id) {


        $this->Institute_model->prof_delete($id);

        $this->add_professors_institute();
    }

    public function add_courses() {

        $this->form_validation->set_rules('course', 'Select Course ', 'trim|required');
        $this->form_validation->set_rules('duration', 'Duration', 'trim|required');
        $this->form_validation->set_rules('for_exam_in', 'Exam_in', 'trim|required');
        $this->form_validation->set_rules('batch_type', 'Batch Type', 'trim|required');
        $this->form_validation->set_rules('date', 'Batch Starts on', 'trim|required');
        $this->form_validation->set_rules('start_time', 'Start Time', 'trim|required');
        $this->form_validation->set_rules('end_time', 'To Time', 'trim|required');
        $this->form_validation->set_rules('faculty', 'Faculty', 'trim|required');
        $this->form_validation->set_rules('fee', 'Fee', 'trim|required');
        $this->form_validation->set_rules('city', 'City', 'trim|required');

        if ($this->form_validation->run() == FALSE) {
            $this->form_validation->set_message('required', 'All Fields Are Required');

            $this->pagination->initialize($this->set_pagination_courses());
            $page_ = ($this->uri->segment(2)) ? $this->uri->segment(2) : 0;
            $data["results"] = $this->Institute_model->get_courses_pagen($this->set_pagination_courses(), $page_);
            $data["pages"] = $this->pagination->create_links();
            $data['institute'] = $this->Institute_model->get_institute_info();
            $data['professor'] = $this->Institute_model->getone_professor();
            //$data['results'] = $this->Institute_model->get_all_courses();            

            $data['courses'] = $this->Institute_model->get_course();
            $data['faculty'] = $this->Institute_model->get_faculty();
            $this->load->view('templates/head');
            $this->load->view('templates/header');
            $this->load->view('institute/add_courses', $data);
        } else {
            $i = $this->input->post('sem_count');
            $course_id = $this->input->post('course');
            $semister = $this->input->post('semister');
            foreach ($semister as $sem) {
                $e = 'subjects'.$sem;
                $subs = $this->input->post($e);
                $subjects = implode(',', $subs);
                $data = array(
                    'inst_id' => $_SESSION['id'],
                    'course_id' => $course_id,
                    'semister_id' => $sem,
                    'subjects' => $subjects
                );
               $this->db->insert('institute_semisters', $data);
            }
           
            $faculty = explode('-', $this->input->post('faculty'));
            //$course = explode('-', $this->input->post('course'));
            //$city = implode( $this->input->post('city'));
            // $id = $this->input->post('id');
            //$location = $this->input->post('location');
            $data = array(
                'inst_id' => $_SESSION['id'],
                'prof_id' => $faculty[1],
                'faculty' => $faculty[0],
                // 'course_id' => $course[0],
                //'course_name' => $course[0],
                //'faculty' => $this->input->post('faculty'),                             
                 'course_id' => $this->input->post('course'),
                'duration' => $this->input->post('duration'),
                'exam_in' => $this->input->post('for_exam_in'),
                'batch_type' => $this->input->post('batch_type'),
                'batch_starts' => $this->input->post('date'),
                'batch_time_from' => $this->input->post('start_time'),
                'batch_time_to' => $this->input->post('end_time'),
                'fee' => $this->input->post('fee'),
                'city' => $this->input->post('city'),
                'location' => $this->input->post('location')
                    //'city' => $city[1],
            );

            $this->Institute_model->add_course($data);
            $this->form_validation->set_message('success', 'Course Added Successfully');
            $data['mesg'] = 'Course Added Successfully';
            //$data['institute'] = $this->Institute_model->get_inst_details_m();
            $this->pagination->initialize($this->set_pagination_courses());
            $page_ = ($this->uri->segment(2)) ? $this->uri->segment(2) : 0;
            $data["results"] = $this->Institute_model->get_courses_pagen($this->set_pagination_courses(), $page_);
            $data["pages"] = $this->pagination->create_links();
            $data['institute'] = $this->Institute_model->get_institute_info();
            $data['professor'] = $this->Institute_model->getone_professor();
            $data['courses'] = $this->Institute_model->get_course();
            $data['faculty'] = $this->Institute_model->get_faculty();
            //$data['results'] = $this->Institute_model->get_all_courses();
            $this->load->view('templates/head');
            $this->load->view('templates/header');
            $this->load->view('institute/add_courses', $data);
        }
    }

    public function course_update() {


        $id = $this->input->post('id');
        $data = array(
            'faculty' => $this->input->post('faculty'),
            //'course_name' => $this->input->post('course'),
            //'duration' => $this->input->post('duration'),
            //'exam_in' => $this->input->post('for_exam_in'),
            //'batch_type' => $this->input->post('batch_type'),
            'batch_starts' => $this->input->post('date'),
            'batch_time_from' => $this->input->post('time_from'),
            'batch_time_to' => $this->input->post('time_to'),
            'fee' => $this->input->post('fee'),
                //'city' => $this->input->post('city')
                //'city' => $city[1],
        );

        $this->db->where('course_id', $id);
        $this->db->update('institute_courses', $data);
        //$this->load->view('templates/header');
        //$this->load->view('account/inst_account_page',$data);
        $this->add_courses_institute();
    }

    public function show_semisters() {

//  
        $this->load->view('institute/show_semisters');
    }

    public function show_subjects() {
        $course = $this->input->post('course');
        $semister = $this->input->post('semister');
        $this->db->where('course_name', $course);
        $this->db->where('semister', $semister);
        //$subject = implode(',', $this->input->post('subject'));
        $var['subjects'] = $this->db->get('all_courses')->row();
        $this->load->view('institute/subjects', $var);
    }

    public function courses_delete($id) {


        $this->Institute_model->course_delete($id);

        $this->add_courses_institute();
    }

    public function institute_profile_view() {

        $this->pagination->initialize($this->set_pagination());
        $page = ($this->uri->segment(3)) ? $this->uri->segment(3) : 0;
        $data["professors"] = $this->Institute_model->get_professors($this->set_pagination(), $page);
        $data["links"] = $this->pagination->create_links();

        //$data['institute'] = $this->Institute_model->get_inst_details_m();
        $data['institute'] = $this->Institute_model->get_institute_info();
        $data['professor'] = $this->Institute_model->getone_professor();
        //$data['images'] = $this->Institute_model->get_images();
        $this->pagination->initialize($this->set_pagination_subs());
        $page_sub = ($this->uri->segment(2)) ? $this->uri->segment(2) : 0;
        $data['subscribed'] = $this->Institute_model->get_subscribed_list($this->set_pagination_subs(), $page_sub);
        $data['subscribe'] = $this->Institute_model->get_timings();
        $data['subscribers'] = $this->Institute_model->get_subscribers();
        //$data['counts'] = $this->Institute_model->count();
        //$data['count'] = $this->Institute_model->count_reviews();
        //$data['professors'] = $this->Institute_model->get_professors();
        $data['courses'] = $this->Institute_model->get_course();
        //$data['reviews'] = $this->Institute_model->get_reviews();
        $data['faculty'] = $this->Institute_model->get_faculty();
        //$this->pagination->initialize($this->set_pagination_courses());
       // $page_ = ($this->uri->segment(2)) ? $this->uri->segment(2) : 0;
       // $data['results'] = $this->Institute_model->get_courses_pagen($this->set_pagination_(), $page_);
        $data['post'] = 'Your Review Posted Successfully..! ';

        $this->load->view('institute/institute_profile', $data);
    }

    public function post_review_popup() {
        
                if(!empty($_SESSION['logged_in']) ){    
                    echo 'true';

                }  else {
                    echo "false";
                }
                
        }
            
    public function post_review(){
                $id =  $this->input->post('id');
                $data = array(
            'inst_id' => $id,
            'student_id' => $_SESSION['student_id'],
            'headline' => $this->input->post('headline'),
            'review' => $this->input->post('review'),
            'rating' => $this->input->post('rating')
        );
            $this->db->insert('reviews', $data);
           // echo $this->db->insert_id();
            redirect(base_url().'index.php/Welcome/institute_profile_view/'.$id.'/'.$_SESSION['student_id'].' ');
            
                
            }
            
            public function post_recmnd_popup() {
        
                if(!empty($_SESSION['logged_in']) ){
                     $id =  $this->input->post('insid');
                     $cid =  $this->input->post('course_id');
                $data = array(
            'inst_id' => $id,
            'course_id' => $cid,
            'student_id' => $_SESSION['student_id'],
            'recommend' => $this->input->post('posttxt'),
            //'pin_status' => $this->input->post('posttxt')
            
        );
               
            $this->db->insert('recommendations', $data);
            
                    echo 'true';

                }  else {
                    echo "false";
                }
                
        }

         public function post_pinned_popup() {
        
                if(!empty($_SESSION['logged_in']) ){
                     $id =  $this->input->post('insid');
                
            $data2 = array(
                'inst_id' => $id,
                'student_id' => $_SESSION['student_id'],
                'pin_status' => $this->input->post('posttxt')
            );
             $this->db->insert('pinned_tbl', $data2);
                    echo 'true';

                }  else {
                    echo "false";
                }
                
        }
        
          public function subscription(){
            
             if(!empty($_SESSION['logged_in']) ){
                     $id =  $this->input->post('insti_id');
                     $c_id =  $this->input->post('cou_id');
                     $p_id =  $this->input->post('prof_id');
                     $data = array(
                         'student_id'   =>  $_SESSION['student_id'],
                         'inst_id'      =>  $id,
                         'course_id'    =>  $c_id,
                         'prof_id'      =>  $p_id
                     );
                     $this->db->insert('subscribers', $data);
                     echo 'true';
                     
             }  else {
                 echo 'false'; 
             }
        
        }
     public function inst_videos(){
         
         $find = 'width="560" height="315" ';
          $data = array(
             'inst_id' =>$this->input->post('inst_id'),
             'videos' =>str_replace($find,'',$this->input->post('videos')),
         );
         $this->db->insert('inst_videos',$data);
         redirect('Institute/load_account_page');
        
       
     }
     
     public function delete_videos($id,$inst_id){
         
        $this->db->where('id', $id);
        $this->db->where('inst_id', $inst_id);
        $this->db->delete('inst_videos');
        $this->load_account_page();
    }
     
     
            
}
        
  


