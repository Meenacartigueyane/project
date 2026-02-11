// HPI 1.7-G
import React, { useRef } from 'react';
import { Link } from 'react-router-dom';
import { motion, useScroll, useTransform, useSpring, useInView } from 'framer-motion';
import { Zap, Trophy, Users, Calendar, ArrowRight, Cpu, Wrench, Lightbulb, ChevronRight, Terminal, Activity, Target } from 'lucide-react';
import Header from '@/components/Header';
import Footer from '@/components/Footer';
import { Button } from '@/components/ui/button';
import { Image } from '@/components/ui/image';

// --- Types ---
interface Stat {
  icon: React.ElementType;
  label: string;
  value: string;
  color: string;
}

interface Highlight {
  icon: React.ElementType;
  title: string;
  description: string;
}

// --- Components ---

const GridBackground = () => (
  <div className="absolute inset-0 pointer-events-none overflow-hidden">
    <div 
      className="absolute inset-0 opacity-[0.03]" 
      style={{
        backgroundImage: `
          linear-gradient(to right, #007BFF 1px, transparent 1px),
          linear-gradient(to bottom, #007BFF 1px, transparent 1px)
        `,
        backgroundSize: '4rem 4rem',
        maskImage: 'radial-gradient(circle at center, black 40%, transparent 100%)'
      }} 
    />
    <div className="absolute top-0 left-1/4 w-px h-full bg-gradient-to-b from-transparent via-primary/20 to-transparent" />
    <div className="absolute top-0 right-1/4 w-px h-full bg-gradient-to-b from-transparent via-primary/20 to-transparent" />
  </div>
);

const TechnicalBracket = ({ className }: { className?: string }) => (
  <svg className={className} width="20" height="20" viewBox="0 0 20 20" fill="none">
    <path d="M1 5V1H5" stroke="currentColor" strokeWidth="2" />
    <path d="M19 5V1H15" stroke="currentColor" strokeWidth="2" />
    <path d="M1 15V19H5" stroke="currentColor" strokeWidth="2" />
    <path d="M19 15V19H15" stroke="currentColor" strokeWidth="2" />
  </svg>
);

export default function HomePage() {
  // --- Canonical Data Sources ---
  const featuredStats: Stat[] = [
    { icon: Zap, label: 'Technical Events', value: '15+', color: 'text-primary' },
    { icon: Users, label: 'Participants', value: '500+', color: 'text-accent-orange' },
    { icon: Trophy, label: 'Prize Pool', value: '$10K+', color: 'text-primary' },
    { icon: Calendar, label: 'Days of Innovation', value: '3', color: 'text-accent-orange' },
  ];

  const highlights: Highlight[] = [
    {
      icon: Cpu,
      title: 'Cutting-Edge Competitions',
      description: 'Participate in robotics, coding, and engineering challenges that push the boundaries of innovation.',
    },
    {
      icon: Wrench,
      title: 'Hands-On Workshops',
      description: 'Learn from industry experts through practical sessions on emerging technologies and tools.',
    },
    {
      icon: Lightbulb,
      title: 'Industry Exposure',
      description: 'Network with professionals and gain insights into real-world engineering applications.',
    },
  ];

  // --- Scroll Hooks ---
  const containerRef = useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: containerRef,
    offset: ["start start", "end end"]
  });

  const heroY = useTransform(scrollYProgress, [0, 0.2], [0, 200]);
  const heroOpacity = useTransform(scrollYProgress, [0, 0.2], [1, 0]);

  return (
    <div ref={containerRef} className="min-h-screen bg-background text-foreground font-paragraph selection:bg-primary/30 selection:text-primary-foreground overflow-clip">
      <style>{`
        .clip-tech-card {
          clip-path: polygon(
            0 0, 
            100% 0, 
            100% calc(100% - 20px), 
            calc(100% - 20px) 100%, 
            0 100%
          );
        }
        .clip-tech-button {
          clip-path: polygon(
            10px 0, 
            100% 0, 
            100% calc(100% - 10px), 
            calc(100% - 10px) 100%, 
            0 100%, 
            0 10px
          );
        }
        .grid-mask {
          mask-image: linear-gradient(to bottom, black 80%, transparent 100%);
        }
      `}</style>

      <Header />

      {/* --- HERO SECTION --- */}
      <section className="relative w-full min-h-screen flex items-center justify-center overflow-hidden pt-20 lg:pt-0">
        <GridBackground />
        
        {/* Animated Background Elements */}
        <div className="absolute inset-0 overflow-hidden pointer-events-none">
          <motion.div 
            style={{ y: heroY }}
            className="absolute top-[-10%] right-[-5%] w-[60vw] h-[60vw] bg-primary/5 rounded-full blur-[100px]" 
          />
          <motion.div 
            style={{ y: useTransform(scrollYProgress, [0, 0.5], [0, -100]) }}
            className="absolute bottom-[-10%] left-[-10%] w-[50vw] h-[50vw] bg-accent-orange/5 rounded-full blur-[100px]" 
          />
        </div>

        <div className="relative z-10 w-full max-w-[120rem] mx-auto px-6 lg:px-12 grid lg:grid-cols-12 gap-12 items-center h-full">
          
          {/* Left Column: Typography & CTA */}
          <motion.div 
            className="lg:col-span-7 flex flex-col justify-center space-y-8 lg:space-y-12"
            style={{ opacity: heroOpacity }}
          >
            {/* Badge */}
            <div className="flex items-center gap-3">
              <div className="h-px w-12 bg-accent-orange" />
              <span className="text-accent-orange font-paragraph text-xs lg:text-sm font-bold tracking-[0.2em] uppercase">
                System Online • 2026 Edition
              </span>
            </div>

            {/* Main Headline */}
            <div className="relative">
              <h1 className="font-heading text-6xl lg:text-8xl xl:text-9xl font-bold leading-[0.9] tracking-tighter">
                <span className="block text-foreground mix-blend-difference">MECHANIC</span>
                <span className="block text-transparent bg-clip-text bg-gradient-to-r from-primary via-white to-primary bg-[length:200%_auto] animate-gradient">
                  FEAST
                </span>
              </h1>
              <TechnicalBracket className="absolute -top-8 -left-8 text-primary/30 w-16 h-16 hidden lg:block" />
              <TechnicalBracket className="absolute -bottom-8 -right-8 text-primary/30 w-16 h-16 rotate-180 hidden lg:block" />
            </div>

            <p className="font-paragraph text-lg lg:text-xl text-foreground/60 max-w-2xl leading-relaxed border-l-2 border-primary/20 pl-6">
              Where precision engineering meets digital innovation. The ultimate technical fest for the architects of tomorrow.
            </p>

            {/* CTA Group */}
            <div className="flex flex-wrap gap-6 items-center">
              <Link to="/registration">
                <Button className="group relative bg-primary hover:bg-primary/90 text-white px-10 py-8 text-lg font-paragraph font-bold clip-tech-button transition-all duration-300 hover:translate-x-1">
                  <span className="relative z-10 flex items-center gap-3">
                    INITIALIZE REGISTRATION
                    <ArrowRight className="w-5 h-5 group-hover:translate-x-1 transition-transform" />
                  </span>
                  <div className="absolute inset-0 bg-white/10 translate-y-full group-hover:translate-y-0 transition-transform duration-300" />
                </Button>
              </Link>
              
              <Link to="/events" className="group flex items-center gap-3 text-foreground/80 hover:text-primary transition-colors px-4 py-2">
                <span className="font-paragraph font-medium tracking-wide">VIEW PROTOCOLS</span>
                <div className="w-8 h-px bg-current group-hover:w-12 transition-all" />
              </Link>
            </div>
          </motion.div>

          {/* Right Column: Visual Composition */}
          <div className="lg:col-span-5 relative h-[50vh] lg:h-[80vh] flex items-center justify-center">
            <div className="relative w-full h-full max-h-[600px] perspective-1000">
              {/* Floating Cards Parallax */}
              <motion.div 
                animate={{ y: [0, -20, 0] }}
                transition={{ duration: 6, repeat: Infinity, ease: "easeInOut" }}
                className="absolute top-0 right-0 z-20 w-64 bg-background/80 backdrop-blur-xl border border-primary/30 p-6 rounded-sm shadow-2xl"
              >
                <div className="flex items-center gap-4 mb-4">
                  <Activity className="text-accent-orange w-6 h-6" />
                  <div className="h-1 w-full bg-primary/20 rounded-full overflow-hidden">
                    <div className="h-full w-2/3 bg-accent-orange animate-pulse" />
                  </div>
                </div>
                <div className="space-y-2">
                  <div className="text-xs text-foreground/50 font-mono">SYSTEM STATUS</div>
                  <div className="text-xl font-heading font-bold">OPTIMAL</div>
                </div>
              </motion.div>

              <motion.div 
                animate={{ y: [0, 30, 0] }}
                transition={{ duration: 8, repeat: Infinity, ease: "easeInOut", delay: 1 }}
                className="absolute bottom-10 left-0 z-20 w-72 bg-background/90 backdrop-blur-xl border-l-4 border-accent-orange p-6 shadow-2xl"
              >
                <div className="flex justify-between items-start mb-2">
                  <Terminal className="text-primary w-6 h-6" />
                  <span className="text-xs font-mono text-primary">CMD_EXEC_01</span>
                </div>
                <div className="font-mono text-sm text-foreground/80">
                  &gt; Initiating launch sequence...<br/>
                  &gt; Loading modules...<br/>
                  &gt; <span className="text-accent-orange animate-pulse">Ready.</span>
                </div>
              </motion.div>

              {/* Main Image Container */}
              <div className="absolute inset-4 lg:inset-12 z-10 clip-tech-card bg-gradient-to-br from-primary/20 to-transparent p-1">
                <div className="w-full h-full bg-background clip-tech-card relative overflow-hidden">
                  <div className="absolute inset-0 bg-primary/10 mix-blend-overlay z-10" />
                  <Image 
                    src="https://static.wixstatic.com/media/56d5ff_f60ba1acc3bc48afae7d03a4a34bd3e4~mv2.png?originWidth=768&originHeight=576"
                    alt="Engineering Innovation"
                    width={800}
                    className="w-full h-full object-cover scale-110 hover:scale-100 transition-transform duration-700"
                  />
                </div>
              </div>
              
              {/* Decorative Grid Behind */}
              <div className="absolute inset-0 border border-primary/10 z-0 transform rotate-3 scale-105" />
            </div>
          </div>
        </div>
      </section>

      {/* --- STATS TICKER SECTION --- */}
      <section className="w-full border-y border-white/5 bg-white/[0.02] backdrop-blur-sm overflow-hidden">
        <div className="max-w-[120rem] mx-auto">
          <div className="grid grid-cols-2 md:grid-cols-4 divide-x divide-white/5">
            {featuredStats.map((stat, index) => (
              <div key={index} className="group relative p-8 lg:p-12 flex flex-col items-center justify-center text-center hover:bg-white/[0.02] transition-colors duration-300">
                <div className={`mb-4 p-3 rounded-full bg-white/5 ${stat.color} group-hover:scale-110 transition-transform duration-300`}>
                  <stat.icon className="w-6 h-6" />
                </div>
                <h3 className="font-heading text-3xl lg:text-4xl font-bold mb-2">{stat.value}</h3>
                <p className="font-paragraph text-xs lg:text-sm text-foreground/50 uppercase tracking-widest">{stat.label}</p>
                <div className="absolute top-0 left-0 w-full h-0.5 bg-gradient-to-r from-transparent via-primary/50 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-500" />
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* --- STICKY HIGHLIGHTS SECTION --- */}
      <section className="relative w-full py-32 bg-background">
        <div className="max-w-[120rem] mx-auto px-6 lg:px-12">
          <div className="grid lg:grid-cols-12 gap-16">
            
            {/* Sticky Header */}
            <div className="lg:col-span-4 relative">
              <div className="sticky top-32 space-y-8">
                <div className="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-accent-orange/30 bg-accent-orange/10 text-accent-orange text-xs font-mono">
                  <span className="relative flex h-2 w-2">
                    <span className="animate-ping absolute inline-flex h-full w-full rounded-full bg-accent-orange opacity-75"></span>
                    <span className="relative inline-flex rounded-full h-2 w-2 bg-accent-orange"></span>
                  </span>
                  CORE MODULES
                </div>
                
                <h2 className="font-heading text-5xl lg:text-6xl font-bold leading-tight">
                  Engineered for <br />
                  <span className="text-primary">Excellence</span>
                </h2>
                
                <p className="font-paragraph text-foreground/60 text-lg leading-relaxed">
                  Our platform is built on three pillars of technical advancement. Explore the modules designed to elevate your engineering potential.
                </p>

                <div className="hidden lg:block pt-8">
                  <svg width="100" height="100" viewBox="0 0 100 100" className="animate-[spin_10s_linear_infinite] opacity-20">
                    <circle cx="50" cy="50" r="48" stroke="currentColor" strokeWidth="1" strokeDasharray="4 4" fill="none" />
                    <circle cx="50" cy="50" r="30" stroke="currentColor" strokeWidth="1" fill="none" />
                    <path d="M50 20 V80 M20 50 H80" stroke="currentColor" strokeWidth="1" />
                  </svg>
                </div>
              </div>
            </div>

            {/* Scrolling Cards */}
            <div className="lg:col-span-8 space-y-8">
              {highlights.map((highlight, index) => (
                <StickyCard key={index} highlight={highlight} index={index} />
              ))}
            </div>
          </div>
        </div>
      </section>

      {/* --- PARALLAX BREAK SECTION --- */}
      <section className="relative w-full h-[80vh] flex items-center justify-center overflow-hidden my-20">
        <div className="absolute inset-0 z-0">
          <div className="absolute inset-0 bg-background/60 z-10" />
          <div className="fixed inset-0 w-full h-full -z-10">
             {/* Note: In a real implementation, background-attachment: fixed works best for simple parallax, 
                 but for smoother effects we'd use scroll-linked transforms. Here we use a simple fixed bg. */}
             <Image 
               src="https://static.wixstatic.com/media/56d5ff_2f80550dbcac4e3bbb99638073a1e542~mv2.png?originWidth=1280&originHeight=640"
               alt="Background Texture"
               className="w-full h-full object-cover opacity-30 grayscale"
             />
          </div>
        </div>

        <div className="relative z-20 max-w-5xl mx-auto px-6 text-center">
          <motion.div
            initial={{ opacity: 0, scale: 0.9 }}
            whileInView={{ opacity: 1, scale: 1 }}
            transition={{ duration: 0.8 }}
            viewport={{ once: true }}
          >
            <Target className="w-16 h-16 text-accent-orange mx-auto mb-8" />
            <h2 className="font-heading text-4xl lg:text-7xl font-bold mb-8 leading-tight">
              "Innovation distinguishes between a leader and a follower."
            </h2>
            <div className="h-1 w-24 bg-primary mx-auto" />
          </motion.div>
        </div>
      </section>

      {/* --- CTA SECTION --- */}
      <section className="relative w-full py-32 overflow-hidden">
        {/* Background Gradients */}
        <div className="absolute inset-0 bg-gradient-to-b from-background via-primary/5 to-background" />
        <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[800px] h-[800px] bg-accent-orange/10 rounded-full blur-[120px] pointer-events-none" />

        <div className="relative z-10 max-w-[100rem] mx-auto px-6 lg:px-12 text-center">
          <div className="inline-block mb-6 px-4 py-1 border border-foreground/10 rounded-full backdrop-blur-md">
            <span className="font-mono text-sm text-foreground/60">EVENT_ID: MF_2026</span>
          </div>
          
          <h2 className="font-heading text-5xl lg:text-8xl font-bold mb-8 tracking-tight">
            READY TO <span className="text-transparent bg-clip-text bg-gradient-to-r from-accent-orange to-red-500">IGNITE?</span>
          </h2>
          
          <p className="font-paragraph text-xl text-foreground/60 max-w-2xl mx-auto mb-12">
            Join the elite league of engineers. Registration closes soon. Secure your spot in the grid.
          </p>

          <div className="flex flex-col sm:flex-row gap-6 justify-center items-center">
            <Link to="/registration" className="w-full sm:w-auto">
              <Button className="w-full sm:w-auto bg-accent-orange hover:bg-accent-orange/90 text-white px-12 py-8 text-xl font-heading font-bold rounded-none clip-tech-button transition-all hover:shadow-[0_0_30px_rgba(255,140,0,0.4)]">
                INITIATE REGISTRATION
              </Button>
            </Link>
            <Link to="/about" className="w-full sm:w-auto">
              <Button variant="outline" className="w-full sm:w-auto border-2 border-foreground/20 hover:border-foreground/50 bg-transparent text-foreground px-12 py-8 text-xl font-heading font-bold rounded-none clip-tech-button">
                SYSTEM SPECS
              </Button>
            </Link>
          </div>
        </div>
      </section>

      <Footer />
    </div>
  );
}

// --- Sub-Components ---

function StickyCard({ highlight, index }: { highlight: Highlight; index: number }) {
  const cardRef = useRef<HTMLDivElement>(null);
  const isInView = useInView(cardRef, { margin: "-20% 0px -20% 0px" });

  return (
    <motion.div
      ref={cardRef}
      initial={{ opacity: 0, y: 50 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, margin: "-100px" }}
      transition={{ duration: 0.5, delay: index * 0.1 }}
      className={`group relative p-1 bg-gradient-to-br ${isInView ? 'from-primary via-primary/50 to-transparent' : 'from-white/10 to-transparent'} transition-all duration-500 rounded-2xl`}
    >
      <div className="relative bg-background/90 backdrop-blur-xl p-8 lg:p-12 rounded-2xl h-full border border-white/5 overflow-hidden">
        {/* Decorative Background Number */}
        <div className="absolute -right-4 -bottom-8 text-[10rem] font-heading font-bold text-white/[0.02] select-none pointer-events-none">
          0{index + 1}
        </div>

        <div className="flex flex-col md:flex-row gap-8 items-start relative z-10">
          <div className={`p-4 rounded-xl bg-white/5 border border-white/10 ${isInView ? 'text-primary' : 'text-foreground/50'} transition-colors duration-500`}>
            <highlight.icon className="w-8 h-8" />
          </div>
          
          <div className="space-y-4">
            <h3 className="font-heading text-2xl lg:text-3xl font-bold group-hover:text-primary transition-colors">
              {highlight.title}
            </h3>
            <p className="font-paragraph text-foreground/60 leading-relaxed text-lg">
              {highlight.description}
            </p>
            
            <div className="pt-4 flex items-center gap-2 text-sm font-mono text-primary/80 opacity-0 group-hover:opacity-100 transition-opacity transform translate-x-[-10px] group-hover:translate-x-0 duration-300">
              <ChevronRight className="w-4 h-4" />
              <span>ACCESS_MODULE</span>
            </div>
          </div>
        </div>
      </div>
    </motion.div>
  );
}
