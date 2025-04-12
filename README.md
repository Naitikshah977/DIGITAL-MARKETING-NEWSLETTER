import { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Button } from '@/components/ui/button';
import { CheckCircle2, Mail } from 'lucide-react';

export default function NewsletterSignup() {
  const [email, setEmail] = useState('');
  const [subscribed, setSubscribed] = useState(false);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState('');

  const handleSubscribe = async () => {
    if (!email) return;
    setLoading(true);
    setError('');

    try {
      const response = await fetch('/api/subscribe', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ email }),
      });

      if (!response.ok) {
        throw new Error('Failed to subscribe');
      }

      setSubscribed(true);
    } catch (err) {
      setError('Subscription failed. Please try again.');
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-br from-yellow-100 via-pink-100 to-purple-200 p-4">
      <Card className="max-w-md w-full p-6 rounded-2xl shadow-2xl bg-white">
        <CardContent>
          <div className="text-center mb-6">
            <h1 className="text-2xl font-bold text-gray-800">Join Our Digital Marketing Newsletter</h1>
            <p className="text-sm text-gray-600 mt-2">
              Get the latest updates, tips, and trends delivered straight to your inbox.
            </p>
          </div>
          {!subscribed ? (
            <div className="flex flex-col gap-4">
              <Input
                type="email"
                placeholder="Enter your email"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
                className="rounded-xl"
              />
              <Button
                onClick={handleSubscribe}
                className="rounded-xl bg-blue-600 hover:bg-blue-700 text-white"
                disabled={loading}
              >
                <Mail className="mr-2 h-4 w-4" /> {loading ? 'Subscribing...' : 'Subscribe'}
              </Button>
              {error && <p className="text-red-500 text-sm">{error}</p>}
            </div>
          ) : (
            <div className="text-green-600 flex items-center justify-center gap-2">
              <CheckCircle2 className="w-5 h-5" />
              <span>Thanks for subscribing!</span>
            </div>
          )}
        </CardContent>
      </Card>
    </div>
  );
}
