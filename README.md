import React, { useState, useEffect, useRef } from 'react';
import { Github, Linkedin, Mail, Eye, Code, Cpu, Zap, Award, GitBranch, Star, GitCommit, TrendingUp, BookOpen, Lightbulb } from 'lucide-react';

const ReadmePreview = () => {
  const [isVisible, setIsVisible] = useState({});
  const [waveAnimation, setWaveAnimation] = useState(true);
  const observerRefs = useRef([]);

  useEffect(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            setIsVisible(prev => ({
              ...prev,
              [entry.target.dataset.section]: true
            }));
          }
        });
      },
      { threshold: 0.1 }
    );

    observerRefs.current.forEach((ref) => {
      if (ref) observer.observe(ref);
    });

    return () => observer.disconnect();
  }, []);

  useEffect(() => {
    const timer = setTimeout(() => setWaveAnimation(false), 3000);
    return () => clearTimeout(timer);
  }, []);

  const projects = [
    {
      title: "8-Bit Kogge-Stone Adder",
      description: "Designed and implemented a high-performance 8-bit parallel prefix adder, optimized for speed by minimizing carry propagation delay. The design was verified through extensive testbenches and synthesized for FPGA implementation to analyze performance metrics.",
      tags: ["Verilog", "Digital Design", "FPGA", "Vivado"],
      icon: <Cpu className="w-8 h-8" />,
      gradient: "from-purple-600 via-pink-600 to-red-600",
      link: "https://github.com/upadhyaypranjal/8-Bit-Kogge-Stone-Adder"
    },
    {
      title: "ESP32 Electronic Voting Machine",
      description: "Developed a secure IoT-based voting system using an ESP32 microcontroller. The system captures votes, prevents tampering, and transmits results in real-time to a central server via the MQTT protocol, showcasing a practical application of embedded systems in secure data handling.",
      tags: ["ESP32", "C++", "Arduino", "IoT", "MQTT"],
      icon: <Zap className="w-8 h-8" />,
      gradient: "from-blue-600 via-cyan-600 to-teal-600",
      link: "https://github.com/upadhyaypranjal/ESP32-based-Electronic-Voting-Machine"
    }
  ];

  const StatCard = ({ title, value, icon, gradient, delay }) => {
    const [count, setCount] = useState(0);
    const targetValue = parseInt(value);

    useEffect(() => {
      if (isVisible.stats) {
        const duration = 2000;
        const steps = 60;
        const increment = targetValue / steps;
        let current = 0;

        const timer = setInterval(() => {
          current += increment;
          if (current >= targetValue) {
            setCount(targetValue);
            clearInterval(timer);
          } else {
            setCount(Math.floor(current));
          }
        }, duration / steps);

        return () => clearInterval(timer);
      }
    }, [isVisible.stats, targetValue]);

    return (
      <div
        className={`transition-all duration-1000 ${
          isVisible.stats
            ? 'opacity-100 translate-y-0 scale-100'
            : 'opacity-0 translate-y-20 scale-95'
        }`}
        style={{ transitionDelay: `${delay}ms` }}
      >
        <div className="bg-gradient-to-br from-gray-800/80 to-gray-900/80 rounded-2xl p-8 backdrop-blur-sm border border-gray-700/50 hover:border-gray-500/50 transition-all duration-500 hover:scale-105 hover:shadow-2xl group relative overflow-hidden">
          <div className={`absolute inset-0 bg-gradient-to-br ${gradient} opacity-0 group-hover:opacity-5 transition-opacity duration-500`}></div>
          
          <div className={`relative inline-flex items-center justify-center w-16 h-16 rounded-xl bg-gradient-to-br ${gradient} mb-4 shadow-lg group-hover:scale-110 transition-transform duration-300`}>
            {icon}
          </div>
          
          <div className="relative">
            <div className={`text-5xl font-bold mb-2 bg-gradient-to-r ${gradient} bg-clip-text text-transparent`}>
              {count}+
            </div>
            <div className="text-gray-400 text-sm uppercase tracking-wider">
              {title}
            </div>
          </div>
        </div>
      </div>
    );
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-900 via-black to-gray-900 text-white overflow-x-hidden">
      {/* Hero Section */}
      <div className="relative overflow-hidden">
        <div className="absolute inset-0 bg-gradient-to-r from-blue-600/20 via-purple-600/20 to-pink-600/20 animate-pulse"></div>
        <div className="container mx-auto px-4 py-16 relative z-10">
          <div className="text-center">
            {/* Animated Hi There with Hand Wave */}
            <div className="mb-8">
              <h2 className="text-4xl md:text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 via-orange-400 to-red-400 inline-flex items-center gap-3 animate-fadeInUp">
                Hi There! 
                <span className={`inline-block text-5xl md:text-6xl ${waveAnimation ? 'animate-wave' : ''}`}>
                  👋
                </span>
              </h2>
            </div>

            <div className="inline-block mb-6 animate-float">
              <div className="w-32 h-32 rounded-full bg-gradient-to-br from-blue-500 via-purple-500 to-pink-500 p-1 shadow-2xl">
                <div className="w-full h-full rounded-full bg-gray-900 flex items-center justify-center text-5xl font-bold">
                  PU
                </div>
              </div>
            </div>
            
            <h1 className="text-6xl md:text-7xl font-bold mb-2 bg-gradient-to-r from-blue-400 via-purple-400 to-pink-400 bg-clip-text text-transparent animate-gradient">
              Pranjal Upadhyay
            </h1>
            
            {/* Animated Education Line */}
            <div className="mb-8 mt-6">
              <div className="inline-block">
                <p className="text-lg md:text-xl text-gray-300 animate-typewriter overflow-hidden whitespace-nowrap border-r-4 border-cyan-400 pr-2 font-mono">
                  B.Tech + M.Tech | Electronics & Communication Engineering
                </p>
              </div>
              <p className="text-sm md:text-base text-cyan-400 mt-2 animate-fadeIn" style={{ animationDelay: '3s' }}>
                IIITDM Kurnool
              </p>
            </div>

            <div className="flex flex-wrap justify-center gap-3 mb-8">
              {["Hardware Designer 💻", "IoT Enthusiast ⚡", "FPGA Developer 🔧"].map((role, i) => (
                <span
                  key={i}
                  className="px-4 py-2 bg-gradient-to-r from-blue-600/30 to-purple-600/30 rounded-full text-sm border border-blue-500/50 backdrop-blur-sm animate-fadeIn"
                  style={{ animationDelay: `${3.5 + i * 0.2}s` }}
                >
                  {role}
                </span>
              ))}
            </div>
            
            <div className="flex justify-center gap-4">
              <a href="https://www.linkedin.com/in/pranjalupadhyay0142/" className="transform hover:scale-110 transition-all duration-300 animate-fadeIn" style={{ animationDelay: '4.2s' }}>
                <div className="p-3 bg-blue-600 rounded-lg shadow-lg hover:shadow-blue-500/50">
                  <Linkedin className="w-6 h-6" />
                </div>
              </a>
              <a href="https://github.com/upadhyaypranjal" className="transform hover:scale-110 transition-all duration-300 animate-fadeIn" style={{ animationDelay: '4.4s' }}>
                <div className="p-3 bg-gray-800 rounded-lg shadow-lg hover:shadow-gray-500/50">
                  <Github className="w-6 h-6" />
                </div>
              </a>
              <a href="mailto:pranjal2004upadhyay@gmail.com" className="transform hover:scale-110 transition-all duration-300 animate-fadeIn" style={{ animationDelay: '4.6s' }}>
                <div className="p-3 bg-red-600 rounded-lg shadow-lg hover:shadow-red-500/50">
                  <Mail className="w-6 h-6" />
                </div>
              </a>
            </div>
          </div>
        </div>
      </div>

      {/* About Me Section */}
      <div className="container mx-auto px-4 py-20">
        <div
          ref={el => observerRefs.current[20] = el}
          data-section="about"
          className={`transition-all duration-1000 ${
            isVisible.about ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'
          }`}
        >
          <div className="text-center mb-16">
            <div className="inline-block relative">
              <div className="absolute inset-0 bg-gradient-to-r from-cyan-400 via-blue-400 to-purple-400 blur-2xl opacity-30 animate-pulse"></div>
              <BookOpen className="w-16 h-16 mx-auto mb-4 text-cyan-400 animate-bounce relative z-10" />
            </div>
            <h2 className="text-6xl md:text-7xl font-bold bg-gradient-to-r from-cyan-400 via-blue-400 to-purple-400 bg-clip-text text-transparent mb-4 animate-gradient">
              About Me
            </h2>
            <div className="w-32 h-1 mx-auto bg-gradient-to-r from-cyan-400 via-blue-400 to-purple-400 rounded-full"></div>
          </div>
          
          <div className="max-w-5xl mx-auto">
            <div className="relative">
              {/* Decorative elements */}
              <div className="absolute -top-10 -left-10 w-40 h-40 bg-cyan-500/10 rounded-full blur-3xl"></div>
              <div className="absolute -bottom-10 -right-10 w-40 h-40 bg-purple-500/10 rounded-full blur-3xl"></div>
              
              <div className="relative bg-gradient-to-br from-gray-800/80 to-gray-900/80 rounded-3xl p-1 backdrop-blur-sm">
                <div className="bg-gradient-to-br from-gray-900/90 to-black/90 rounded-3xl p-10">
                  <div className="space-y-8">
                    {/* Education Card */}
                    <div className="group relative">
                      <div className="absolute inset-0 bg-gradient-to-r from-cyan-600/20 to-blue-600/20 rounded-2xl blur-xl group-hover:blur-2xl transition-all duration-500"></div>
                      <div className="relative flex items-start gap-6 bg-gradient-to-br from-gray-800/50 to-gray-900/50 p-6 rounded-2xl border border-cyan-500/30 group-hover:border-cyan-500/60 transition-all duration-500">
                        <div className="relative">
                          <div className="absolute inset-0 bg-gradient-to-br from-cyan-600 to-blue-600 rounded-xl blur-md opacity-50 group-hover:opacity-100 transition-opacity"></div>
                          <div className="relative p-4 bg-gradient-to-br from-cyan-600 to-blue-600 rounded-xl group-hover:scale-110 transition-transform flex-shrink-0 shadow-xl">
                            <Award className="w-8 h-8" />
                          </div>
                        </div>
                        <div className="flex-1">
                          <h3 className="text-2xl font-bold text-cyan-400 mb-3 group-hover:text-cyan-300 transition-colors">Education</h3>
                          <p className="text-gray-200 leading-relaxed text-lg mb-2">
                            B.Tech + M.Tech Dual Degree in Electronics & Communication Engineering
                          </p>
                          <p className="text-gray-400 text-base flex items-center gap-2">
                            <span className="inline-block w-2 h-2 bg-cyan-400 rounded-full animate-pulse"></span>
                            Indian Institute of Information Technology Design and Manufacturing, Kurnool
                          </p>
                        </div>
                      </div>
                    </div>

                    {/* Core Interests Card */}
                    <div className="group relative">
                      <div className="absolute inset-0 bg-gradient-to-r from-purple-600/20 to-pink-600/20 rounded-2xl blur-xl group-hover:blur-2xl transition-all duration-500"></div>
                      <div className="relative flex items-start gap-6 bg-gradient-to-br from-gray-800/50 to-gray-900/50 p-6 rounded-2xl border border-purple-500/30 group-hover:border-purple-500/60 transition-all duration-500">
                        <div className="relative">
                          <div className="absolute inset-0 bg-gradient-to-br from-purple-600 to-pink-600 rounded-xl blur-md opacity-50 group-hover:opacity-100 transition-opacity"></div>
                          <div className="relative p-4 bg-gradient-to-br from-purple-600 to-pink-600 rounded-xl group-hover:scale-110 transition-transform flex-shrink-0 shadow-xl">
                            <Cpu className="w-8 h-8" />
                          </div>
                        </div>
                        <div className="flex-1">
                          <h3 className="text-2xl font-bold text-purple-400 mb-3 group-hover:text-purple-300 transition-colors">Core Interests</h3>
                          <p className="text-gray-200 leading-relaxed text-lg">
                            VLSI Design, FPGA Development, and Embedded IoT Solutions
                          </p>
                          <div className="flex flex-wrap gap-2 mt-3">
                            {["VLSI", "FPGA", "IoT"].map((interest, i) => (
                              <span key={i} className="px-3 py-1 bg-purple-500/20 border border-purple-500/50 rounded-full text-sm text-purple-300">
                                {interest}
                              </span>
                            ))}
                          </div>
                        </div>
                      </div>
                    </div>

                    {/* Currently Learning Card */}
                    <div className="group relative">
                      <div className="absolute inset-0 bg-gradient-to-r from-green-600/20 to-teal-600/20 rounded-2xl blur-xl group-hover:blur-2xl transition-all duration-500"></div>
                      <div className="relative flex items-start gap-6 bg-gradient-to-br from-gray-800/50 to-gray-900/50 p-6 rounded-2xl border border-green-500/30 group-hover:border-green-500/60 transition-all duration-500">
                        <div className="relative">
                          <div className="absolute inset-0 bg-gradient-to-br from-green-600 to-teal-600 rounded-xl blur-md opacity-50 group-hover:opacity-100 transition-opacity"></div>
                          <div className="relative p-4 bg-gradient-to-br from-green-600 to-teal-600 rounded-xl group-hover:scale-110 transition-transform flex-shrink-0 shadow-xl">
                            <Lightbulb className="w-8 h-8" />
                          </div>
                        </div>
                        <div className="flex-1">
                          <h3 className="text-2xl font-bold text-green-400 mb-3 group-hover:text-green-300 transition-colors">Currently Learning</h3>
                          <p className="text-gray-200 leading-relaxed text-lg">
                            RISC-V Architecture & System-on-Chip (SoC) Design
                          </p>
                          <div className="mt-3 flex items-center gap-2">
                            <div className="w-full bg-gray-700 rounded-full h-2">
                              <div className="bg-gradient-to-r from-green-500 to-teal-500 h-2 rounded-full animate-progress" style={{ width: '65%' }}></div>
                            </div>
                            <span className="text-green-400 text-sm font-semibold">65%</span>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      {/* Featured Projects */}
      <div className="container mx-auto px-4 py-20">
        <div
          ref={el => observerRefs.current[0] = el}
          data-section="projects"
          className={`text-center mb-16 transition-all duration-1000 ${
            isVisible.projects ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'
          }`}
        >
          <div className="inline-block relative">
            <div className="absolute inset-0 bg-gradient-to-r from-yellow-400 via-orange-400 to-red-400 blur-2xl opacity-30 animate-pulse"></div>
            <Award className="w-16 h-16 mx-auto mb-4 text-yellow-400 animate-bounce relative z-10" />
          </div>
          <h2 className="text-6xl md:text-7xl font-bold bg-gradient-to-r from-yellow-400 via-orange-400 to-red-400 bg-clip-text text-transparent animate-gradient">
            Featured Projects
          </h2>
          <div className="w-32 h-1 mx-auto mt-4 bg-gradient-to-r from-yellow-400 via-orange-400 to-red-400 rounded-full"></div>
        </div>

        <div className="grid md:grid-cols-2 gap-8 max-w-6xl mx-auto">
          {projects.map((project, index) => (
            <div
              key={index}
              ref={el => observerRefs.current[index + 1] = el}
              data-section={`project-${index}`}
              className={`transition-all duration-1000 ${
                isVisible[`project-${index}`]
                  ? 'opacity-100 translate-y-0'
                  : 'opacity-0 translate-y-20'
              }`}
              style={{ transitionDelay: `${index * 200}ms` }}
            >
              <a href={project.link} className="block group h-full">
                <div className="relative h-full bg-gradient-to-br from-gray-800/50 to-gray-900/50 rounded-3xl p-8 backdrop-blur-sm border border-gray-700/50 hover:border-gray-600 transition-all duration-500 hover:scale-105 hover:shadow-2xl overflow-hidden">
                  {/* Animated background gradient */}
                  <div className={`absolute inset-0 bg-gradient-to-br ${project.gradient} opacity-0 group-hover:opacity-10 transition-opacity duration-500`}></div>
                  
                  {/* Icon with gradient background */}
                  <div className={`relative inline-flex items-center justify-center w-20 h-20 rounded-2xl bg-gradient-to-br ${project.gradient} mb-6 shadow-lg group-hover:scale-110 group-hover:rotate-6 transition-all duration-300`}>
                    <div className="text-white">{project.icon}</div>
                  </div>

                  <h3 className="text-3xl font-bold mb-4 group-hover:text-transparent group-hover:bg-clip-text group-hover:bg-gradient-to-r group-hover:from-blue-400 group-hover:to-purple-400 transition-all duration-300">
                    {project.title}
                  </h3>
                  
                  <p className="text-gray-400 text-base mb-6 leading-relaxed">
                    {project.description}
                  </p>

                  <div className="flex flex-wrap gap-2">
                    {project.tags.map((tag, i) => (
                      <span
                        key={i}
                        className={`px-4 py-2 text-sm rounded-full bg-gradient-to-r ${project.gradient} bg-opacity-20 border border-current opacity-70 group-hover:opacity-100 transition-all duration-300 group-hover:scale-105`}
                      >
                        {tag}
                      </span>
                    ))}
                  </div>

                  {/* Hover effect overlay */}
                  <div className="absolute top-6 right-6 opacity-0 group-hover:opacity-100 transition-all duration-300 transform group-hover:scale-110">
                    <Github className="w-8 h-8 text-gray-400" />
                  </div>
                </div>
              </a>
            </div>
          ))}
        </div>
      </div>

      {/* GitHub Statistics */}
      <div className="container mx-auto px-4 py-20">
        <div
          ref={el => observerRefs.current[10] = el}
          data-section="stats"
          className={`text-center mb-16 transition-all duration-1000 ${
            isVisible.stats ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'
          }`}
        >
          <div className="inline-block relative">
            <div className="absolute inset-0 bg-gradient-to-r from-green-400 via-blue-400 to-purple-400 blur-2xl opacity-30 animate-pulse"></div>
            <TrendingUp className="w-16 h-16 mx-auto mb-4 text-green-400 animate-bounce relative z-10" />
          </div>
          <h2 className="text-6xl md:text-7xl font-bold bg-gradient-to-r from-green-400 via-blue-400 to-purple-400 bg-clip-text text-transparent animate-gradient">
            GitHub Statistics
          </h2>
          <div className="w-32 h-1 mx-auto mt-4 bg-gradient-to-r from-green-400 via-blue-400 to-purple-400 rounded-full"></div>
        </div>

        <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-6 max-w-7xl mx-auto">
          <StatCard
            title="Repositories"
            value="25"
            icon={<GitBranch className="w-8 h-8 text-white" />}
            gradient="from-blue-600 to-cyan-600"
            delay={0}
          />
          <StatCard
            title="Contributions"
            value="500"
            icon={<GitCommit className="w-8 h-8 text-white" />}
            gradient="from-purple-600 to-pink-600"
            delay={200}
          />
          <StatCard
            title="Stars Earned"
            icon={<Star className="w-8 h-8 text-white" />}
            value="50"
            gradient="from-yellow-600 to-orange-600"
            delay={400}
          />
          <StatCard
            title="Projects"
            value="15"
            icon={<Code className="w-8 h-8 text-white" />}
            gradient="from-green-600 to-teal-600"
            delay={600}
          />
        </div>
      </div>

      <style jsx>{`
        @keyframes float {
          0%, 100% { transform: translateY(0px); }
          50% { transform: translateY(-20px); }
        }
        @keyframes gradient {
          0%, 100% { background-position: 0% 50%; }
          50% { background-position: 100% 50%; }
        }
        @keyframes fadeIn {
          from { opacity: 0; transform: translateY(10px); }
          to { opacity: 1; transform: translateY(0); }
        }
        @keyframes fadeInUp {
          from { opacity: 0; transform: translateY(30px); }
          to { opacity: 1; transform: translateY(0); }
        }
        @keyframes wave {
          0% { transform: rotate(0deg); }
          10% { transform: rotate(14deg); }
          20% { transform: rotate(-8deg); }
          30% { transform: rotate(14deg); }
          40% { transform: rotate(-4deg); }
          50% { transform: rotate(10deg); }
          60% { transform: rotate(0deg); }
          100% { transform: rotate(0deg); }
        }
        @keyframes typewriter {
          from { width: 0; }
          to { width: 100%; }
        }
        @keyframes progress {
          from { width: 0%; }
          to { width: 65%; }
        }
        .animate-float {
          animation: float 3s ease-in-out infinite;
        }
        .animate-gradient {
          background-size: 200% 200%;
          animation: gradient 3s ease infinite;
        }
        .animate-fadeIn {
          animation: fadeIn 0.8s ease-out forwards;
          opacity: 0;
        }
        .animate-fadeInUp {
          animation: fadeInUp 1s ease-out;
        }
        .animate-wave {
          animation: wave 2.5s ease-in-out;
          transform-origin: 70% 70%;
          display: inline-block;
        }
        .animate-typewriter {
          animation: typewriter 3s steps(50) forwards;
          display: inline-block;
        }
        .animate-progress {
          animation: progress 2s ease-out forwards;
        }
      `}</style>
    </div>
  );
};

export default ReadmePreview;
