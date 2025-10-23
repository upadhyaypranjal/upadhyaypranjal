import React, { useState, useEffect, useRef } from 'react';
import { Github, Linkedin, Mail, Cpu, Zap, Award, GitBranch, Star, GitCommit, TrendingUp, BookOpen, Lightbulb, Code } from 'lucide-react';

const ReadmePreview = () => {
  const [isVisible, setIsVisible] = useState({});
  const [waveAnimation, setWaveAnimation] = useState(true);
  const [gitHubStats, setGitHubStats] = useState({
    repos: 0,
    stars: 0,
    contributions: 0,
    projects: 0
  });
  const observerRefs = useRef([]);

  useEffect(() => {
    // Intersection Observer
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
    // Stop wave after 3 seconds
    const timer = setTimeout(() => setWaveAnimation(false), 3000);
    return () => clearTimeout(timer);
  }, []);

  useEffect(() => {
    // Fetch GitHub stats dynamically
    fetch("https://api.github.com/users/upadhyaypranjal")
      .then(res => res.json())
      .then(data => {
        setGitHubStats({
          repos: data.public_repos || 0,
          stars: 50, // GitHub API needs extra call for stars
          contributions: 500, // Contributions need GraphQL or manual value
          projects: 15 // Can be set manually
        });
      });
  }, []);

  const projects = [
    {
      title: "8-Bit Kogge-Stone Adder",
      description: "Designed and implemented a high-performance 8-bit parallel prefix adder, optimized for speed by minimizing carry propagation delay. Verified through testbenches and synthesized for FPGA implementation.",
      tags: ["Verilog", "Digital Design", "FPGA", "Vivado"],
      icon: <Cpu className="w-8 h-8" />,
      gradient: "from-purple-600 via-pink-600 to-red-600",
      link: "https://github.com/upadhyaypranjal/8-Bit-Kogge-Stone-Adder"
    },
    {
      title: "ESP32 Electronic Voting Machine",
      description: "Secure IoT voting system using ESP32. Captures votes, prevents tampering, and transmits results in real-time via MQTT to a central server.",
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
        <div className="container mx-auto px-4 py-16 relative z-10 text-center">
          <h2 className="text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 via-orange-400 to-red-400 inline-flex items-center justify-center gap-3 animate-fadeInUp">
            Hi There! 
            <span className={`inline-block text-6xl ${waveAnimation ? 'animate-wave' : ''}`}>👋</span>
          </h2>

          <div className="inline-block my-8 animate-float">
            <div className="w-32 h-32 rounded-full bg-gradient-to-br from-blue-500 via-purple-500 to-pink-500 p-1 shadow-2xl">
              <div className="w-full h-full rounded-full bg-gray-900 flex items-center justify-center text-5xl font-bold">
                PU
              </div>
            </div>
          </div>

          <h1 className="text-7xl font-bold mb-8 bg-gradient-to-r from-blue-400 via-purple-400 to-pink-400 bg-clip-text text-transparent animate-gradient">
            Pranjal Upadhyay
          </h1>

          <div className="flex justify-center gap-4">
            <a href="https://www.linkedin.com/in/pranjalupadhyay0142/" className="transform hover:scale-110 transition-all duration-300">
              <div className="p-3 bg-blue-600 rounded-lg shadow-lg hover:shadow-blue-500/50">
                <Linkedin className="w-6 h-6" />
              </div>
            </a>
            <a href="https://github.com/upadhyaypranjal" className="transform hover:scale-110 transition-all duration-300">
              <div className="p-3 bg-gray-800 rounded-lg shadow-lg hover:shadow-gray-500/50">
                <Github className="w-6 h-6" />
              </div>
            </a>
            <a href="mailto:pranjal2004upadhyay@gmail.com" className="transform hover:scale-110 transition-all duration-300">
              <div className="p-3 bg-red-600 rounded-lg shadow-lg hover:shadow-red-500/50">
                <Mail className="w-6 h-6" />
              </div>
            </a>
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
                  <div className={`absolute inset-0 bg-gradient-to-br ${project.gradient} opacity-0 group-hover:opacity-10 transition-opacity duration-500`}></div>
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
            value={gitHubStats.repos}
            icon={<GitBranch className="w-8 h-8 text-white" />}
            gradient="from-blue-600 to-cyan-600"
            delay={0}
          />
          <StatCard
            title="Contributions"
            value={gitHubStats.contributions}
            icon={<GitCommit className="w-8 h-8 text-white" />}
            gradient="from-purple-600 to-pink-600"
            delay={200}
          />
          <StatCard
            title="Stars Earned"
            icon={<Star className="w-8 h-8 text-white" />}
            value={gitHubStats.stars}
            gradient="from-yellow-600 to-orange-600"
            delay={400}
          />
          <StatCard
            title="Projects"
            value={gitHubStats.projects}
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
        }
        .animate-fadeInUp {
          animation: fadeInUp 1s ease-out forwards;
        }
        .animate-wave {
          animation: wave 2.5s ease-in-out infinite;
          transform-origin: 70% 70%;
          display: inline-block;
        }
        .animate-typewriter {
          animation: typewriter 3s steps(50) forwards;
          display: inline-block;
          white-space: nowrap;
          overflow: hidden;
        }
        .animate-progress {
          animation: progress 2s ease-out forwards;
        }
      `}</style>
    </div>
  );
};

export default ReadmePreview;
