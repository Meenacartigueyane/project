import { useState, useEffect } from 'react';
import { motion } from 'framer-motion';
import { useForm } from 'react-hook-form';
import { CheckCircle } from 'lucide-react';
import Header from '@/components/Header';
import Footer from '@/components/Footer';
import { BaseCrudService } from '@/integrations';
import { Registrations, TechnicalEvents, Workshops } from '@/entities';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select';

interface RegistrationForm {
  studentName: string;
  studentId: string;
  studentEmail: string;
  studentPhone: string;
  registeredItemName: string;
  registeredItemType: string;
}

export default function RegistrationPage() {
  const [events, setEvents] = useState<TechnicalEvents[]>([]);
  const [workshops, setWorkshops] = useState<Workshops[]>([]);
  const [selectedType, setSelectedType] = useState<string>('');
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [isSuccess, setIsSuccess] = useState(false);

  const { register, handleSubmit, formState: { errors }, reset, setValue } = useForm<RegistrationForm>();

  useEffect(() => {
    const loadData = async () => {
      try {
        const [eventsResult, workshopsResult] = await Promise.all([
          BaseCrudService.getAll<TechnicalEvents>('technicalevents', {}, { limit: 100 }),
          BaseCrudService.getAll<Workshops>('workshops', {}, { limit: 100 })
        ]);
        setEvents(eventsResult.items);
        setWorkshops(workshopsResult.items);
      } catch (error) {
        console.error('Error loading data:', error);
      }
    };
    loadData();
  }, []);

  const onSubmit = async (data: RegistrationForm) => {
    try {
      setIsSubmitting(true);
      await BaseCrudService.create<Registrations>('registrations', {
        _id: crypto.randomUUID(),
        studentName: data.studentName,
        studentId: data.studentId,
        studentEmail: data.studentEmail,
        studentPhone: data.studentPhone,
        registeredItemName: data.registeredItemName,
        registeredItemType: data.registeredItemType,
        registrationDate: new Date().toISOString(),
      });
      setIsSuccess(true);
      reset();
      setSelectedType('');
    } catch (error) {
      console.error('Error submitting registration:', error);
    } finally {
      setIsSubmitting(false);
    }
  };

  const items = selectedType === 'Event' ? events : selectedType === 'Workshop' ? workshops : [];

  return (
    <div className="min-h-screen bg-background text-foreground">
      <Header />

      {/* Hero Section */}
      <section className="relative w-full py-24 overflow-hidden">
        <div className="absolute inset-0 bg-gradient-to-br from-primary/10 via-background to-accent-orange/10" />
        <div className="absolute inset-0 opacity-10">
          <div className="absolute inset-0" style={{
            backgroundImage: 'linear-gradient(rgba(0, 123, 255, 0.3) 1px, transparent 1px), linear-gradient(90deg, rgba(0, 123, 255, 0.3) 1px, transparent 1px)',
            backgroundSize: '50px 50px'
          }} />
        </div>

        <div className="relative z-10 max-w-[100rem] mx-auto px-6 lg:px-12">
          <motion.div
            initial={{ opacity: 0, y: 30 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.6 }}
            className="text-center max-w-4xl mx-auto"
          >
            <h1 className="font-heading text-5xl lg:text-7xl font-bold text-foreground mb-6">
              Event <span className="text-primary">Registration</span>
            </h1>
            <p className="font-paragraph text-lg lg:text-xl text-foreground/80 leading-relaxed">
              Register for technical events and workshops to secure your spot at Mechanic Feast.
            </p>
          </motion.div>
        </div>
      </section>

      {/* Registration Form */}
      <section className="relative w-full py-16">
        <div className="max-w-[800px] mx-auto px-6 lg:px-12">
          {isSuccess ? (
            <motion.div
              initial={{ opacity: 0, scale: 0.9 }}
              animate={{ opacity: 1, scale: 1 }}
              transition={{ duration: 0.5 }}
              className="bg-background/60 backdrop-blur-sm border border-primary/40 rounded-xl p-12 text-center"
            >
              <div className="bg-primary/20 w-20 h-20 rounded-full flex items-center justify-center mx-auto mb-6">
                <CheckCircle className="h-10 w-10 text-primary" />
              </div>
              <h2 className="font-heading text-3xl font-bold text-foreground mb-4">
                Registration Successful!
              </h2>
              <p className="font-paragraph text-foreground/70 mb-8">
                Thank you for registering. You will receive a confirmation email shortly.
              </p>
              <Button
                onClick={() => setIsSuccess(false)}
                className="bg-primary hover:bg-primary/90 text-primary-foreground font-paragraph font-semibold"
              >
                Register Another
              </Button>
            </motion.div>
          ) : (
            <motion.div
              initial={{ opacity: 0, y: 30 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.6 }}
              className="bg-background/60 backdrop-blur-sm border border-primary/20 rounded-xl p-8 lg:p-12"
            >
              <form onSubmit={handleSubmit(onSubmit)} className="space-y-6">
                {/* Student Name */}
                <div className="space-y-2">
                  <Label htmlFor="studentName" className="font-paragraph text-foreground">
                    Full Name *
                  </Label>
                  <Input
                    id="studentName"
                    {...register('studentName', { required: 'Name is required' })}
                    className="bg-background/60 border-primary/30 text-foreground font-paragraph"
                    placeholder="Enter your full name"
                  />
                  {errors.studentName && (
                    <p className="text-destructive text-sm font-paragraph">{errors.studentName.message}</p>
                  )}
                </div>

                {/* Student ID */}
                <div className="space-y-2">
                  <Label htmlFor="studentId" className="font-paragraph text-foreground">
                    Student ID *
                  </Label>
                  <Input
                    id="studentId"
                    {...register('studentId', { required: 'Student ID is required' })}
                    className="bg-background/60 border-primary/30 text-foreground font-paragraph"
                    placeholder="Enter your student ID"
                  />
                  {errors.studentId && (
                    <p className="text-destructive text-sm font-paragraph">{errors.studentId.message}</p>
                  )}
                </div>

                {/* Email */}
                <div className="space-y-2">
                  <Label htmlFor="studentEmail" className="font-paragraph text-foreground">
                    Email Address *
                  </Label>
                  <Input
                    id="studentEmail"
                    type="email"
                    {...register('studentEmail', {
                      required: 'Email is required',
                      pattern: {
                        value: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i,
                        message: 'Invalid email address'
                      }
                    })}
                    className="bg-background/60 border-primary/30 text-foreground font-paragraph"
                    placeholder="your.email@example.com"
                  />
                  {errors.studentEmail && (
                    <p className="text-destructive text-sm font-paragraph">{errors.studentEmail.message}</p>
                  )}
                </div>

                {/* Phone */}
                <div className="space-y-2">
                  <Label htmlFor="studentPhone" className="font-paragraph text-foreground">
                    Phone Number *
                  </Label>
                  <Input
                    id="studentPhone"
                    {...register('studentPhone', { required: 'Phone number is required' })}
                    className="bg-background/60 border-primary/30 text-foreground font-paragraph"
                    placeholder="+1 (234) 567-890"
                  />
                  {errors.studentPhone && (
                    <p className="text-destructive text-sm font-paragraph">{errors.studentPhone.message}</p>
                  )}
                </div>

                {/* Registration Type */}
                <div className="space-y-2">
                  <Label htmlFor="registeredItemType" className="font-paragraph text-foreground">
                    Registration Type *
                  </Label>
                  <Select
                    onValueChange={(value) => {
                      setSelectedType(value);
                      setValue('registeredItemType', value);
                      setValue('registeredItemName', '');
                    }}
                    value={selectedType}
                  >
                    <SelectTrigger className="bg-background/60 border-primary/30 text-foreground font-paragraph">
                      <SelectValue placeholder="Select type" />
                    </SelectTrigger>
                    <SelectContent>
                      <SelectItem value="Event">Technical Event</SelectItem>
                      <SelectItem value="Workshop">Workshop</SelectItem>
                    </SelectContent>
                  </Select>
                  <input type="hidden" {...register('registeredItemType', { required: 'Type is required' })} />
                  {errors.registeredItemType && (
                    <p className="text-destructive text-sm font-paragraph">{errors.registeredItemType.message}</p>
                  )}
                </div>

                {/* Item Selection */}
                {selectedType && (
                  <div className="space-y-2">
                    <Label htmlFor="registeredItemName" className="font-paragraph text-foreground">
                      Select {selectedType} *
                    </Label>
                    <Select
                      onValueChange={(value) => setValue('registeredItemName', value)}
                    >
                      <SelectTrigger className="bg-background/60 border-primary/30 text-foreground font-paragraph">
                        <SelectValue placeholder={`Select ${selectedType.toLowerCase()}`} />
                      </SelectTrigger>
                      <SelectContent>
                        {items.map((item) => (
                          <SelectItem key={item._id} value={item.title || ''}>
                            {item.title}
                          </SelectItem>
                        ))}
                      </SelectContent>
                    </Select>
                    <input type="hidden" {...register('registeredItemName', { required: 'Selection is required' })} />
                    {errors.registeredItemName && (
                      <p className="text-destructive text-sm font-paragraph">{errors.registeredItemName.message}</p>
                    )}
                  </div>
                )}

                {/* Submit Button */}
                <div className="pt-6">
                  <Button
                    type="submit"
                    disabled={isSubmitting}
                    className="w-full bg-primary hover:bg-primary/90 text-primary-foreground font-paragraph font-semibold py-6"
                  >
                    {isSubmitting ? 'Submitting...' : 'Complete Registration'}
                  </Button>
                </div>
              </form>
            </motion.div>
          )}
        </div>
      </section>

      <Footer />
    </div>
  );
}
