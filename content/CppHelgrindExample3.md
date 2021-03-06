# ([C++](Cpp.md)) ![Wt](PicWt.png)![Qt Creator](PicQtCreator.png)![Lubuntu](PicLubuntu.png)![Web](PicWeb.png) [helgrind example 3: Hello Wt](CppHelgrindExample3.md)

This [helgrind](CppHelgrind.md) example tests a slightly adapted
version of ['Hello Wt' using Qt Creator under
Ubuntu](CppHelloWtQtCreatorUbuntu.md) for possible problems.

-   [Download the Qt Creator project
    'CppHelgrindExample3' (zip)](CppHelgrindExample3.md)

The original version was adapted to let the program quit. So, I added a
[Wt::WPushButton](CppWPushButton.md) that calls
[std::exit](CppStdExit.md).


## main.cpp

```c++
/*
* Copyright (C) 2008 Emweb bvba, Heverlee, Belgium.
*
* See the LICENSE file for terms of use.
*/

#include <Wt/WApplication>
#include <Wt/WBreak>
#include <Wt/WContainerWidget>
#include <Wt/WLineEdit>
#include <Wt/WPushButton>
#include <Wt/WText>

#include <boost/version.hpp>

using namespace Wt;

/*
* A simple hello world application class which demonstrates how to react
* to events, read input, and give feed-back.
*/
class HelloApplication : public WApplication
{
public:
  HelloApplication(const WEnvironment& env);

private:
  WLineEdit *nameEdit_;
  WText *greeting_;

  void greet();
  void quit(); //Added by RJCB
};

/*
* The env argument contains information about the new session, and
* the initial request. It must be passed to the WApplication
* constructor so it is typically also an argument for your custom
* application constructor.
*/
HelloApplication::HelloApplication(const WEnvironment& env)
  : WApplication(env)
{
  setTitle("Hello world");                               // application title

  root()->addWidget(new WText("Your name, please ? "));  // show some text
  nameEdit_ = new WLineEdit(root());                     // allow text input
  nameEdit_->setFocus();                                 // give focus

  WPushButton *b = new WPushButton("Greet me.", root()); // create a button
  b->setMargin(5, Left);                                 // add 5 pixels margin

  root()->addWidget(new WBreak());                       // insert a line break

  greeting_ = new WText(root());                         // empty text


  root()->addWidget(new WBreak());                         // RJCB
  WPushButton * const q = new WPushButton("Quit", root()); // RJCB
  q->clicked().connect(this, &HelloApplication::quit);     // RJCB

  /*
   * Connect signals with slots
   *
   * - simple Wt-way
   */
  b->clicked().connect(this, &HelloApplication::greet);

  /*
   * - using an arbitrary function object (binding values with boost::bind())
   */
  nameEdit_->enterPressed().connect
    (boost::bind(&HelloApplication::greet, this));
}

void HelloApplication::greet()
{
  /*
   * Update the text, using text input into the nameEdit_ field.
   */
  greeting_->setText("Hello there, " + nameEdit_->text());
}

//Method added by RJCB
void HelloApplication::quit()
{
  std::exit(0);
}

WApplication *createApplication(const WEnvironment& env)
{
  /*
   * You could read information from the environment to decide whether
   * the user has permission to start a new application
   */
  return new HelloApplication(env);
}

int main(int argc, char **argv)
{
  /*
   * Your main method may set up some shared resources, but should then
   * start the server application (FastCGI or httpd) that starts listening
   * for requests, and handles all of the application life cycles.
   *
   * The last argument to WRun specifies the function that will instantiate
   * new application objects. That function is executed when a new user surfs
   * to the Wt application, and after the library has negotiated browser
   * support. The function should return a newly instantiated application
   * object.
   */
  return WRun(argc, argv, &createApplication);
}
``` 

## helgrind.sh

```
#!/bin/sh
valgrind --tool=helgrind --log-file=helgrind.txt ../CppHelgrindExample3-build-desktop/./CppHelgrindExample3 --docroot=. --http-address=0.0.0.0 --http-port=8080
``` 

## memcheck.sh

```
#!/bin/sh
valgrind --leak-check=full -v --show-reachable=yes --log-file=memcheck.txt ../CppHelgrindExample3-build-desktop/./CppHelgrindExample3 --docroot=. --http-address=0.0.0.0 --http-port=8080
``` 

## helgrind.txt

helgrind.txt is the output file created by [helgrind](CppHelgrind.md):
 
```
==17884== Helgrind, a thread error detector
==17884== Copyright (C) 2007-2010, and GNU GPL'd, by OpenWorks LLP et al.
==17884== Using Valgrind-3.6.1 and LibVEX; rerun with -h for copyright info
==17884== Command: ../CppHelgrindExample3-build-desktop/./CppHelgrindExample3 --docroot=. --http-address=0.0.0.0 --http-port=8080
==17884== Parent PID: 17883
==17884==
==17884== Thread #3 was created
==17884==    at 0x4A9A728: clone (clone.S:111)
==17884==    by 0x48894B5: pthread_create@@GLIBC_2.1 (createthread.c:256)
==17884==    by 0x4026E2D: pthread_create_WRK (hg_intercepts.c:257)
==17884==    by 0x4026F8B: pthread_create@* (hg_intercepts.c:288)
==17884==    by 0x4B48D7C: boost::thread::start_thread() (thread.cpp:185)
==17884==    by 0x45C2C6D: Wt::WServer::start() (thread.hpp:191)
==17884==    by 0x45C3A38: Wt::WRun(int, char**, boost::function<Wt::WApplication* ()(Wt::WEnvironment const&)>) (WServer.C:492)
==17884==    by 0x804ADED: main (in /home/richel/Projects/Website/CppHelgrindExample3-build-desktop/CppHelgrindExample3)
==17884==
==17884== Thread #2 was created
==17884==    at 0x4A9A728: clone (clone.S:111)
==17884==    by 0x48894B5: pthread_create@@GLIBC_2.1 (createthread.c:256)
==17884==    by 0x4026E2D: pthread_create_WRK (hg_intercepts.c:257)
==17884==    by 0x4026F8B: pthread_create@* (hg_intercepts.c:288)
==17884==    by 0x4B48D7C: boost::thread::start_thread() (thread.cpp:185)
==17884==    by 0x45C2C6D: Wt::WServer::start() (thread.hpp:191)
==17884==    by 0x45C3A38: Wt::WRun(int, char**, boost::function<Wt::WApplication* ()(Wt::WEnvironment const&)>) (WServer.C:492)
==17884==    by 0x804ADED: main (in /home/richel/Projects/Website/CppHelgrindExample3-build-desktop/CppHelgrindExample3)
==17884==
==17884== Possible data race during read of size 4 at 0x4b5318c by thread #3
==17884==    at 0x4B48E44: _ZN5boost9call_onceIPFvvEEEvRNS_9once_flagET_.clone.101 (once.hpp:49)
==17884==    by 0x4B48F96: boost::detail::set_current_thread_data(boost::detail::thread_data_base*) (thread.cpp:104)
==17884==    by 0x4B4901D: thread_proxy (thread.cpp:117)
==17884==    by 0x4026F60: mythread_wrapper (hg_intercepts.c:221)
==17884==    by 0x4888E98: start_thread (pthread_create.c:304)
==17884==    by 0x4A9A73D: clone (clone.S:130)
==17884==  This conflicts with a previous write of size 4 by thread #2
==17884==    at 0x4B48F3D: _ZN5boost9call_onceIPFvvEEEvRNS_9once_flagET_.clone.101 (once.hpp:72)
==17884==    by 0x4B48F96: boost::detail::set_current_thread_data(boost::detail::thread_data_base*) (thread.cpp:104)
==17884==    by 0x4B4901D: thread_proxy (thread.cpp:117)
==17884==    by 0x4026F60: mythread_wrapper (hg_intercepts.c:221)
==17884==    by 0x4888E98: start_thread (pthread_create.c:304)
==17884==    by 0x4A9A73D: clone (clone.S:130)
==17884==
==17884== Possible data race during read of size 4 at 0x4b53188 by thread #3
==17884==    at 0x4B48E4A: _ZN5boost9call_onceIPFvvEEEvRNS_9once_flagET_.clone.101 (once.hpp:49)
==17884==    by 0x4B48F96: boost::detail::set_current_thread_data(boost::detail::thread_data_base*) (thread.cpp:104)
==17884==    by 0x4B4901D: thread_proxy (thread.cpp:117)
==17884==    by 0x4026F60: mythread_wrapper (hg_intercepts.c:221)
==17884==    by 0x4888E98: start_thread (pthread_create.c:304)
==17884==    by 0x4A9A73D: clone (clone.S:130)
==17884==  This conflicts with a previous write of size 4 by thread #2
==17884==    at 0x4B48F43: _ZN5boost9call_onceIPFvvEEEvRNS_9once_flagET_.clone.101 (once.hpp:72)
==17884==    by 0x4B48F96: boost::detail::set_current_thread_data(boost::detail::thread_data_base*) (thread.cpp:104)
==17884==    by 0x4B4901D: thread_proxy (thread.cpp:117)
==17884==    by 0x4026F60: mythread_wrapper (hg_intercepts.c:221)
==17884==    by 0x4888E98: start_thread (pthread_create.c:304)
==17884==    by 0x4A9A73D: clone (clone.S:130)
==17884==
==17884== Possible data race during read of size 4 at 0x4b531f4 by thread #3
==17884==    at 0x4B4E85F: boost::detail::get_once_per_thread_epoch() (once.cpp:40)
==17884==    by 0x4B48E54: _ZN5boost9call_onceIPFvvEEEvRNS_9once_flagET_.clone.101 (once.hpp:50)
==17884==    by 0x4B48F96: boost::detail::set_current_thread_data(boost::detail::thread_data_base*) (thread.cpp:104)
==17884==    by 0x4B4901D: thread_proxy (thread.cpp:117)
==17884==    by 0x4026F60: mythread_wrapper (hg_intercepts.c:221)
==17884==    by 0x4888E98: start_thread (pthread_create.c:304)
==17884==    by 0x4A9A73D: clone (clone.S:130)
==17884==  This conflicts with a previous write of size 4 by thread #2
==17884==    at 0x488E34D: pthread_key_create (pthread_key_create.c:44)
==17884==    by 0x4B4E7F9: create_epoch_tss_key (once.cpp:32)
==17884==    by 0x488E99F: pthread_once (pthread_once.S:122)
==17884==    by 0x4B48E54: _ZN5boost9call_onceIPFvvEEEvRNS_9once_flagET_.clone.101 (once.hpp:50)
==17884==    by 0x4B48F96: boost::detail::set_current_thread_data(boost::detail::thread_data_base*) (thread.cpp:104)
==17884==    by 0x4B4901D: thread_proxy (thread.cpp:117)
==17884==    by 0x4026F60: mythread_wrapper (hg_intercepts.c:221)
==17884==    by 0x4888E98: start_thread (pthread_create.c:304)
==17884==
==17884== Thread #9 was created
==17884==    at 0x4A9A728: clone (clone.S:111)
==17884==    by 0x48894B5: pthread_create@@GLIBC_2.1 (createthread.c:256)
==17884==    by 0x4026E2D: pthread_create_WRK (hg_intercepts.c:257)
==17884==    by 0x4026F8B: pthread_create@* (hg_intercepts.c:288)
==17884==    by 0x4B48D7C: boost::thread::start_thread() (thread.cpp:185)
==17884==    by 0x45C2C6D: Wt::WServer::start() (thread.hpp:191)
==17884==    by 0x45C3A38: Wt::WRun(int, char**, boost::function<Wt::WApplication* ()(Wt::WEnvironment const&)>) (WServer.C:492)
==17884==    by 0x804ADED: main (in /home/richel/Projects/Website/CppHelgrindExample3-build-desktop/CppHelgrindExample3)
==17884==
==17884== Possible data race during write of size 4 at 0x613f638 by thread #9
==17884==    at 0x43B9069: Wt::WebSession::setState(Wt::WebSession::State, int) (WebSession.C:222)
==17884==    by 0x43B914E: Wt::WebSession::setLoaded() (WebSession.C:209)
==17884==    by 0x43C0994: Wt::WebSession::handleRequest(Wt::WebSession::Handler&) (WebSession.C:1214)
==17884==    by 0x43D86A3: Wt::WebController::handleAsyncRequest(Wt::WebRequest*) (WebController.C:638)
==17884==    by 0x43D9703: Wt::WebController::handleRequest(Wt::WebRequest*) (WebController.C:512)
==17884==    by 0x45BE959: Wt::WServer::handleRequest(Wt::WebRequest*) (WServer.C:372)
==17884==    by 0x45C690D: http::server::WtReply::consumeRequestBody(char const*, char const*, http::server::Request::State) (WtReply.C:187)
==17884==    by 0x45C7CDB: http::server::WtReply::consumeData(char const*, char const*, http::server::Request::State) (WtReply.C:81)
==17884==    by 0x45889DF: http::server::RequestParser::parseBody(http::server::Request&, boost::shared_ptr<http::server::Reply>, char const*&, char const*) (RequestParser.C:137)
==17884==    by 0x456F58D: http::server::Connection::handleReadBody() (Connection.C:172)
==17884==    by 0x4571A8B: http::server::Connection::handleReadRequest0() (Connection.C:120)
==17884==    by 0x4571C89: http::server::Connection::handleReadRequest(boost::system::error_code const&, unsigned int) (Connection.C:154)
==17884==  This conflicts with a previous read of size 4 by thread #2
==17884==    at 0x43D85BD: Wt::WebController::handleAsyncRequest(Wt::WebRequest*) (WebController.C:590)
==17884==    by 0x43D9703: Wt::WebController::handleRequest(Wt::WebRequest*) (WebController.C:512)
==17884==    by 0x45BE959: Wt::WServer::handleRequest(Wt::WebRequest*) (WServer.C:372)
==17884==    by 0x45C690D: http::server::WtReply::consumeRequestBody(char const*, char const*, http::server::Request::State) (WtReply.C:187)
==17884==    by 0x45C7CDB: http::server::WtReply::consumeData(char const*, char const*, http::server::Request::State) (WtReply.C:81)
==17884==    by 0x45889DF: http::server::RequestParser::parseBody(http::server::Request&, boost::shared_ptr<http::server::Reply>, char const*&, char const*) (RequestParser.C:137)
==17884==    by 0x456F58D: http::server::Connection::handleReadBody() (Connection.C:172)
==17884==    by 0x4571A8B: http::server::Connection::handleReadRequest0() (Connection.C:120)
==17884==  Address 0x613f638 is 144 bytes inside a block of size 1472 alloc'd
==17884==    at 0x402647D: operator new(unsigned int) (vg_replace_malloc.c:255)
==17884==    by 0x43D89ED: Wt::WebController::handleAsyncRequest(Wt::WebRequest*) (WebController.C:613)
==17884==    by 0x43D9703: Wt::WebController::handleRequest(Wt::WebRequest*) (WebController.C:512)
==17884==    by 0x45BE959: Wt::WServer::handleRequest(Wt::WebRequest*) (WServer.C:372)
==17884==    by 0x45C690D: http::server::WtReply::consumeRequestBody(char const*, char const*, http::server::Request::State) (WtReply.C:187)
==17884==    by 0x45C7CDB: http::server::WtReply::consumeData(char const*, char const*, http::server::Request::State) (WtReply.C:81)
==17884==    by 0x45889DF: http::server::RequestParser::parseBody(http::server::Request&, boost::shared_ptr<http::server::Reply>, char const*&, char const*) (RequestParser.C:137)
==17884==    by 0x456F58D: http::server::Connection::handleReadBody() (Connection.C:172)
==17884==    by 0x4571A8B: http::server::Connection::handleReadRequest0() (Connection.C:120)
==17884==    by 0x4571C89: http::server::Connection::handleReadRequest(boost::system::error_code const&, unsigned int) (Connection.C:154)
==17884==    by 0x45B6071: boost::asio::detail::handler_queue::handler_wrapper<boost::asio::detail::binder2<boost::_bi::bind_t<void, boost::_mfi::mf2<void, http::server::Connection, boost::system::error_code const&, unsigned int>, boost::_bi::list3<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)(), boost::arg<2> (*)()> >, boost::system::error_code, unsigned int> >::do_call(boost::asio::detail::handler_queue::handler*) (mem_fn_template.hpp:271)
==17884==    by 0x4598445: boost::asio::detail::task_io_service<boost::asio::detail::epoll_reactor<false> >::run(boost::system::error_code&) (handler_queue.hpp:39)
==17884==
==17884== Possible data race during write of size 4 at 0x614c8f0 by thread #2
==17884==    at 0x4928DFE: std::string::_M_mutate(unsigned int, unsigned int, unsigned int) (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x45C64D7: http::server::WtReply::nextContentBuffer() (basic_string.h:787)
==17884==    by 0x457F722: http::server::Reply::encodeNextContentBuffer(std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >&, int&, int&) (Reply.C:527)
==17884==    by 0x4580BD1: http::server::Reply::nextBuffers(std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >&) (Reply.C:369)
==17884==    by 0x45713FA: http::server::Connection::startWriteResponse() (Connection.C:201)
==17884==    by 0x457123F: http::server::Connection::handleWriteResponse() (Connection.C:225)
==17884==    by 0x4571385: http::server::Connection::handleWriteResponse(boost::system::error_code const&) (Connection.C:257)
==17884==    by 0x45BC10F: boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >::operator()(boost::system::error_code const&, unsigned int) (mem_fn_template.hpp:156)
==17884==    by 0x45BCBD1: boost::asio::detail::handler_queue::handler_wrapper<boost::asio::detail::binder2<boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >, boost::system::error_code, unsigned int> >::do_call(boost::asio::detail::handler_queue::handler*) (bind_handler.hpp:96)
==17884==    by 0x4598445: boost::asio::detail::task_io_service<boost::asio::detail::epoll_reactor<false> >::run(boost::system::error_code&) (handler_queue.hpp:39)
==17884==    by 0x458E4ED: http::server::Server::run() (io_service.ipp:68)
==17884==    by 0x45C413A: Wt::WServerImpl::runThread(Wt::WServer*) (WServer.C:123)
==17884==  This conflicts with a previous read of size 4 by thread #9
==17884==    at 0x4927D5E: std::string::swap(std::string&) (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x45C64EF: http::server::WtReply::nextContentBuffer() (WtReply.C:329)
==17884==    by 0x457F722: http::server::Reply::encodeNextContentBuffer(std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >&, int&, int&) (Reply.C:527)
==17884==    by 0x4580BD1: http::server::Reply::nextBuffers(std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >&) (Reply.C:369)
==17884==    by 0x45713FA: http::server::Connection::startWriteResponse() (Connection.C:201)
==17884==    by 0x457123F: http::server::Connection::handleWriteResponse() (Connection.C:225)
==17884==    by 0x4571385: http::server::Connection::handleWriteResponse(boost::system::error_code const&) (Connection.C:257)
==17884==    by 0x45BC10F: boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >::operator()(boost::system::error_code const&, unsigned int) (mem_fn_template.hpp:156)
==17884==  Address 0x614c8f0 is 8 bytes inside a block of size 65 alloc'd
==17884==    at 0x402647D: operator new(unsigned int) (vg_replace_malloc.c:255)
==17884==    by 0x49289F7: std::string::_Rep::_S_create(unsigned int, unsigned int, std::allocator<char> const&) (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x4169873: char* std::string::_S_construct<char*>(char*, char*, std::allocator<char> const&, std::forward_iterator_tag) (basic_string.tcc:138)
==17884==    by 0x457D674: http::server::HTTPRequest::flush(Wt::WebRequest::ResponseState, boost::function<void ()()>) (basic_string.h:1649)
==17884==    by 0x43C041E: Wt::WebSession::handleRequest(Wt::WebSession::Handler&) (WebSession.C:1254)
==17884==    by 0x43D86A3: Wt::WebController::handleAsyncRequest(Wt::WebRequest*) (WebController.C:638)
==17884==    by 0x43D9703: Wt::WebController::handleRequest(Wt::WebRequest*) (WebController.C:512)
==17884==    by 0x45BE959: Wt::WServer::handleRequest(Wt::WebRequest*) (WServer.C:372)
==17884==    by 0x45C690D: http::server::WtReply::consumeRequestBody(char const*, char const*, http::server::Request::State) (WtReply.C:187)
==17884==    by 0x45C7CDB: http::server::WtReply::consumeData(char const*, char const*, http::server::Request::State) (WtReply.C:81)
==17884==    by 0x45889DF: http::server::RequestParser::parseBody(http::server::Request&, boost::shared_ptr<http::server::Reply>, char const*&, char const*) (RequestParser.C:137)
==17884==    by 0x456F58D: http::server::Connection::handleReadBody() (Connection.C:172)
==17884==
==17884== Thread #10 was created
==17884==    at 0x4A9A728: clone (clone.S:111)
==17884==    by 0x48894B5: pthread_create@@GLIBC_2.1 (createthread.c:256)
==17884==    by 0x4026E2D: pthread_create_WRK (hg_intercepts.c:257)
==17884==    by 0x4026F8B: pthread_create@* (hg_intercepts.c:288)
==17884==    by 0x4B48D7C: boost::thread::start_thread() (thread.cpp:185)
==17884==    by 0x45C2C6D: Wt::WServer::start() (thread.hpp:191)
==17884==    by 0x45C3A38: Wt::WRun(int, char**, boost::function<Wt::WApplication* ()(Wt::WEnvironment const&)>) (WServer.C:492)
==17884==    by 0x804ADED: main (in /home/richel/Projects/Website/CppHelgrindExample3-build-desktop/CppHelgrindExample3)
==17884==
==17884== Possible data race during write of size 4 at 0x49816f0 by thread #2
==17884==    at 0x48EF2B9: std::locale::locale(std::locale::_Impl*) (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x48F1505: std::locale::classic() (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x42679FB: std::basic_string<char, std::char_traits<char>, std::allocator<char> > boost::posix_time::to_simple_string_type<char>(boost::posix_time::ptime) (date_formatting.hpp:85)
==17884==    by 0x4266507: Wt::WLogEntry::operator<<(Wt::WLogger::TimeStamp const&) (time_formatters.hpp:184)
==17884==    by 0x457F35E: http::server::Reply::logReply(Wt::WLogger&) (Reply.C:469)
==17884==    by 0x4571111: http::server::Connection::handleWriteResponse() (Connection.C:232)
==17884==    by 0x4571385: http::server::Connection::handleWriteResponse(boost::system::error_code const&) (Connection.C:257)
==17884==    by 0x45BC10F: boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >::operator()(boost::system::error_code const&, unsigned int) (mem_fn_template.hpp:156)
==17884==    by 0x45BCBD1: boost::asio::detail::handler_queue::handler_wrapper<boost::asio::detail::binder2<boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >, boost::system::error_code, unsigned int> >::do_call(boost::asio::detail::handler_queue::handler*) (bind_handler.hpp:96)
==17884==    by 0x4598445: boost::asio::detail::task_io_service<boost::asio::detail::epoll_reactor<false> >::run(boost::system::error_code&) (handler_queue.hpp:39)
==17884==    by 0x458E4ED: http::server::Server::run() (io_service.ipp:68)
==17884==    by 0x45C413A: Wt::WServerImpl::runThread(Wt::WServer*) (WServer.C:123)
==17884==  This conflicts with a previous write of size 4 by thread #10
==17884==    at 0x48EF2B9: std::locale::locale(std::locale::_Impl*) (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x48F1505: std::locale::classic() (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x42679FB: std::basic_string<char, std::char_traits<char>, std::allocator<char> > boost::posix_time::to_simple_string_type<char>(boost::posix_time::ptime) (date_formatting.hpp:85)
==17884==    by 0x4266507: Wt::WLogEntry::operator<<(Wt::WLogger::TimeStamp const&) (time_formatters.hpp:184)
==17884==    by 0x43B8DC0: Wt::WebSession::log(std::string const&) const (WebSession.C:131)
==17884==    by 0x43C3CB1: Wt::WebSession::~WebSession() (WebSession.C:171)
==17884==    by 0x436F078: boost::detail::sp_counted_impl_p<Wt::WebSession>::dispose() (checked_delete.hpp:34)
==17884==    by 0x804B8F7: boost::detail::shared_count::~shared_count() (in /home/richel/Projects/Website/CppHelgrindExample3-build-desktop/CppHelgrindExample3)
==17884==
==17884== Possible data race during write of size 4 at 0x6144d40 by thread #2
==17884==    at 0x45C5106: http::server::WtReply::~WtReply() (WtReply.C:63)
==17884==    by 0x45C56B1: http::server::WtReply::~WtReply() (WtReply.C:72)
==17884==    by 0x4587AE7: boost::detail::sp_counted_impl_p<http::server::WtReply>::dispose() (checked_delete.hpp:34)
==17884==    by 0x804B8F7: boost::detail::shared_count::~shared_count() (in /home/richel/Projects/Website/CppHelgrindExample3-build-desktop/CppHelgrindExample3)
==17884==    by 0x4571179: http::server::Connection::handleWriteResponse() (shared_ptr.hpp:169)
==17884==    by 0x4571385: http::server::Connection::handleWriteResponse(boost::system::error_code const&) (Connection.C:257)
==17884==    by 0x45BC10F: boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >::operator()(boost::system::error_code const&, unsigned int) (mem_fn_template.hpp:156)
==17884==    by 0x45BCBD1: boost::asio::detail::handler_queue::handler_wrapper<boost::asio::detail::binder2<boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >, boost::system::error_code, unsigned int> >::do_call(boost::asio::detail::handler_queue::handler*) (bind_handler.hpp:96)
==17884==    by 0x4598445: boost::asio::detail::task_io_service<boost::asio::detail::epoll_reactor<false> >::run(boost::system::error_code&) (handler_queue.hpp:39)
==17884==    by 0x458E4ED: http::server::Server::run() (io_service.ipp:68)
==17884==    by 0x45C413A: Wt::WServerImpl::runThread(Wt::WServer*) (WServer.C:123)
==17884==    by 0x45C4041: boost::detail::thread_data<boost::_bi::bind_t<void, boost::_mfi::mf1<void, Wt::WServerImpl, Wt::WServer*>, boost::_bi::list2<boost::_bi::value<Wt::WServerImpl*>, boost::_bi::value<Wt::WServer*> > > >::run() (mem_fn_template.hpp:165)
==17884==  This conflicts with a previous read of size 4 by thread #10
==17884==    at 0x45889ED: http::server::RequestParser::parseBody(http::server::Request&, boost::shared_ptr<http::server::Reply>, char const*&, char const*) (RequestParser.C:139)
==17884==    by 0x456F58D: http::server::Connection::handleReadBody() (Connection.C:172)
==17884==    by 0x4571A8B: http::server::Connection::handleReadRequest0() (Connection.C:120)
==17884==    by 0x4571C89: http::server::Connection::handleReadRequest(boost::system::error_code const&, unsigned int) (Connection.C:154)
==17884==    by 0x45B6071: boost::asio::detail::handler_queue::handler_wrapper<boost::asio::detail::binder2<boost::_bi::bind_t<void, boost::_mfi::mf2<void, http::server::Connection, boost::system::error_code const&, unsigned int>, boost::_bi::list3<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)(), boost::arg<2> (*)()> >, boost::system::error_code, unsigned int> >::do_call(boost::asio::detail::handler_queue::handler*) (mem_fn_template.hpp:271)
==17884==    by 0x4598445: boost::asio::detail::task_io_service<boost::asio::detail::epoll_reactor<false> >::run(boost::system::error_code&) (handler_queue.hpp:39)
==17884==    by 0x458E4ED: http::server::Server::run() (io_service.ipp:68)
==17884==    by 0x45C413A: Wt::WServerImpl::runThread(Wt::WServer*) (WServer.C:123)
==17884==  Address 0x6144d40 is 0 bytes inside a block of size 476 alloc'd
==17884==    at 0x402647D: operator new(unsigned int) (vg_replace_malloc.c:255)
==17884==    by 0x4586C67: http::server::RequestHandler::handleRequest(http::server::Request&) (RequestHandler.C:145)
==17884==    by 0x45719FB: http::server::Connection::handleReadRequest0() (Connection.C:116)
==17884==    by 0x4571C89: http::server::Connection::handleReadRequest(boost::system::error_code const&, unsigned int) (Connection.C:154)
==17884==    by 0x45B6071: boost::asio::detail::handler_queue::handler_wrapper<boost::asio::detail::binder2<boost::_bi::bind_t<void, boost::_mfi::mf2<void, http::server::Connection, boost::system::error_code const&, unsigned int>, boost::_bi::list3<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)(), boost::arg<2> (*)()> >, boost::system::error_code, unsigned int> >::do_call(boost::asio::detail::handler_queue::handler*) (mem_fn_template.hpp:271)
==17884==    by 0x4598445: boost::asio::detail::task_io_service<boost::asio::detail::epoll_reactor<false> >::run(boost::system::error_code&) (handler_queue.hpp:39)
==17884==    by 0x458E4ED: http::server::Server::run() (io_service.ipp:68)
==17884==    by 0x45C413A: Wt::WServerImpl::runThread(Wt::WServer*) (WServer.C:123)
==17884==    by 0x45C4041: boost::detail::thread_data<boost::_bi::bind_t<void, boost::_mfi::mf1<void, Wt::WServerImpl, Wt::WServer*>, boost::_bi::list2<boost::_bi::value<Wt::WServerImpl*>, boost::_bi::value<Wt::WServer*> > > >::run() (mem_fn_template.hpp:165)
==17884==    by 0x4B49028: thread_proxy (thread.cpp:120)
==17884==    by 0x4026F60: mythread_wrapper (hg_intercepts.c:221)
==17884==    by 0x4888E98: start_thread (pthread_create.c:304)
==17884==
==17884== Thread #8 was created
==17884==    at 0x4A9A728: clone (clone.S:111)
==17884==    by 0x48894B5: pthread_create@@GLIBC_2.1 (createthread.c:256)
==17884==    by 0x4026E2D: pthread_create_WRK (hg_intercepts.c:257)
==17884==    by 0x4026F8B: pthread_create@* (hg_intercepts.c:288)
==17884==    by 0x4B48D7C: boost::thread::start_thread() (thread.cpp:185)
==17884==    by 0x45C2C6D: Wt::WServer::start() (thread.hpp:191)
==17884==    by 0x45C3A38: Wt::WRun(int, char**, boost::function<Wt::WApplication* ()(Wt::WEnvironment const&)>) (WServer.C:492)
==17884==    by 0x804ADED: main (in /home/richel/Projects/Website/CppHelgrindExample3-build-desktop/CppHelgrindExample3)
==17884==
==17884== Thread #11 was created
==17884==    at 0x4A9A728: clone (clone.S:111)
==17884==    by 0x48894B5: pthread_create@@GLIBC_2.1 (createthread.c:256)
==17884==    by 0x4026E2D: pthread_create_WRK (hg_intercepts.c:257)
==17884==    by 0x4026F8B: pthread_create@* (hg_intercepts.c:288)
==17884==    by 0x4B48D7C: boost::thread::start_thread() (thread.cpp:185)
==17884==    by 0x45C2C6D: Wt::WServer::start() (thread.hpp:191)
==17884==    by 0x45C3A38: Wt::WRun(int, char**, boost::function<Wt::WApplication* ()(Wt::WEnvironment const&)>) (WServer.C:492)
==17884==    by 0x804ADED: main (in /home/richel/Projects/Website/CppHelgrindExample3-build-desktop/CppHelgrindExample3)
==17884==
==17884== Possible data race during read of size 4 at 0x497fe8c by thread #8
==17884==    at 0x491C438: std::basic_ostream<char, std::char_traits<char> >& std::__ostream_insert<char, std::char_traits<char> >(std::basic_ostream<char, std::char_traits<char> >&, char const*, int) (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x4265C7A: Wt::WLogger::addLine(std::string const&) const (basic_string.h:2605)
==17884==    by 0x426621F: Wt::WLogEntry::~WLogEntry() (WLogger.C:27)
==17884==    by 0x457F49E: http::server::Reply::logReply(Wt::WLogger&) (Reply.C:464)
==17884==    by 0x4571111: http::server::Connection::handleWriteResponse() (Connection.C:232)
==17884==    by 0x4571385: http::server::Connection::handleWriteResponse(boost::system::error_code const&) (Connection.C:257)
==17884==    by 0x45BC10F: boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >::operator()(boost::system::error_code const&, unsigned int) (mem_fn_template.hpp:156)
==17884==    by 0x45BCBD1: boost::asio::detail::handler_queue::handler_wrapper<boost::asio::detail::binder2<boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >, boost::system::error_code, unsigned int> >::do_call(boost::asio::detail::handler_queue::handler*) (bind_handler.hpp:96)
==17884==    by 0x4598445: boost::asio::detail::task_io_service<boost::asio::detail::epoll_reactor<false> >::run(boost::system::error_code&) (handler_queue.hpp:39)
==17884==    by 0x458E4ED: http::server::Server::run() (io_service.ipp:68)
==17884==    by 0x45C413A: Wt::WServerImpl::runThread(Wt::WServer*) (WServer.C:123)
==17884==    by 0x45C4041: boost::detail::thread_data<boost::_bi::bind_t<void, boost::_mfi::mf1<void, Wt::WServerImpl, Wt::WServer*>, boost::_bi::list2<boost::_bi::value<Wt::WServerImpl*>, boost::_bi::value<Wt::WServer*> > > >::run() (mem_fn_template.hpp:165)
==17884==  This conflicts with a previous write of size 4 by thread #11
==17884==    at 0x491C53C: std::basic_ostream<char, std::char_traits<char> >& std::__ostream_insert<char, std::char_traits<char> >(std::basic_ostream<char, std::char_traits<char> >&, char const*, int) (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x4265C7A: Wt::WLogger::addLine(std::string const&) const (basic_string.h:2605)
==17884==    by 0x426621F: Wt::WLogEntry::~WLogEntry() (WLogger.C:27)
==17884==    by 0x457F49E: http::server::Reply::logReply(Wt::WLogger&) (Reply.C:464)
==17884==    by 0x4571111: http::server::Connection::handleWriteResponse() (Connection.C:232)
==17884==    by 0x4571385: http::server::Connection::handleWriteResponse(boost::system::error_code const&) (Connection.C:257)
==17884==    by 0x45BC10F: boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >::operator()(boost::system::error_code const&, unsigned int) (mem_fn_template.hpp:156)
==17884==    by 0x45BCBD1: boost::asio::detail::handler_queue::handler_wrapper<boost::asio::detail::binder2<boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >, boost::system::error_code, unsigned int> >::do_call(boost::asio::detail::handler_queue::handler*) (bind_handler.hpp:96)
==17884==
==17884== Possible data race during write of size 4 at 0x497fe8c by thread #8
==17884==    at 0x491C53C: std::basic_ostream<char, std::char_traits<char> >& std::__ostream_insert<char, std::char_traits<char> >(std::basic_ostream<char, std::char_traits<char> >&, char const*, int) (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x4265C7A: Wt::WLogger::addLine(std::string const&) const (basic_string.h:2605)
==17884==    by 0x426621F: Wt::WLogEntry::~WLogEntry() (WLogger.C:27)
==17884==    by 0x457F49E: http::server::Reply::logReply(Wt::WLogger&) (Reply.C:464)
==17884==    by 0x4571111: http::server::Connection::handleWriteResponse() (Connection.C:232)
==17884==    by 0x4571385: http::server::Connection::handleWriteResponse(boost::system::error_code const&) (Connection.C:257)
==17884==    by 0x45BC10F: boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >::operator()(boost::system::error_code const&, unsigned int) (mem_fn_template.hpp:156)
==17884==    by 0x45BCBD1: boost::asio::detail::handler_queue::handler_wrapper<boost::asio::detail::binder2<boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >, boost::system::error_code, unsigned int> >::do_call(boost::asio::detail::handler_queue::handler*) (bind_handler.hpp:96)
==17884==    by 0x4598445: boost::asio::detail::task_io_service<boost::asio::detail::epoll_reactor<false> >::run(boost::system::error_code&) (handler_queue.hpp:39)
==17884==    by 0x458E4ED: http::server::Server::run() (io_service.ipp:68)
==17884==    by 0x45C413A: Wt::WServerImpl::runThread(Wt::WServer*) (WServer.C:123)
==17884==    by 0x45C4041: boost::detail::thread_data<boost::_bi::bind_t<void, boost::_mfi::mf1<void, Wt::WServerImpl, Wt::WServer*>, boost::_bi::list2<boost::_bi::value<Wt::WServerImpl*>, boost::_bi::value<Wt::WServer*> > > >::run() (mem_fn_template.hpp:165)
==17884==  This conflicts with a previous write of size 4 by thread #11
==17884==    at 0x491C53C: std::basic_ostream<char, std::char_traits<char> >& std::__ostream_insert<char, std::char_traits<char> >(std::basic_ostream<char, std::char_traits<char> >&, char const*, int) (in /usr/lib/i386-linux-gnu/libstdc++.so.6.0.14)
==17884==    by 0x4265C7A: Wt::WLogger::addLine(std::string const&) const (basic_string.h:2605)
==17884==    by 0x426621F: Wt::WLogEntry::~WLogEntry() (WLogger.C:27)
==17884==    by 0x457F49E: http::server::Reply::logReply(Wt::WLogger&) (Reply.C:464)
==17884==    by 0x4571111: http::server::Connection::handleWriteResponse() (Connection.C:232)
==17884==    by 0x4571385: http::server::Connection::handleWriteResponse(boost::system::error_code const&) (Connection.C:257)
==17884==    by 0x45BC10F: boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >::operator()(boost::system::error_code const&, unsigned int) (mem_fn_template.hpp:156)
==17884==    by 0x45BCBD1: boost::asio::detail::handler_queue::handler_wrapper<boost::asio::detail::binder2<boost::asio::detail::write_handler<boost::asio::basic_stream_socket<boost::asio::ip::tcp, boost::asio::stream_socket_service<boost::asio::ip::tcp> >, std::vector<boost::asio::const_buffer, std::allocator<boost::asio::const_buffer> >, boost::asio::detail::transfer_all_t, boost::_bi::bind_t<void, boost::_mfi::mf1<void, http::server::Connection, boost::system::error_code const&>, boost::_bi::list2<boost::_bi::value<boost::shared_ptr<http::server::Connection> >, boost::arg<1> (*)()> > >, boost::system::error_code, unsigned int> >::do_call(boost::asio::detail::handler_queue::handler*) (bind_handler.hpp:96)
==17884==
==17884==
==17884== For counts of detected and suppressed errors, rerun with: -v
==17884== Use --history-level=approx or =none to gain increased speed, at
==17884== the cost of reduced accuracy of conflicting-access information
==17884== ERROR SUMMARY: 35 errors from 9 contexts (suppressed: 9621 from 566)
```

35 errors!
