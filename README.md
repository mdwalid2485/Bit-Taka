# Bit-Taka
Bit Taka is a Crypto Currency Based on bangladeshi Currency Taka and Bit Coin.
[This is a Sample Code to Study]
import { useState } from 'react'
import { Trash, Plus } from 'lucide-react'
import { Button } from "/components/ui/button"
import { Card, CardContent, CardHeader, CardTitle } from "/components/ui/card"
import { Input } from "/components/ui/input"
import { Label } from "/components/ui/label"

type Cryptocurrency = {
  id: string
  name: string
  symbol: string
  amount: number
  price: number
}

export default function CryptoPortfolio() {
  const [cryptos, setCryptos] = useState<Cryptocurrency[]>([
    { id: '1', name: 'Bitcoin', symbol: 'BTC', amount: 0.5, price: 50000 },
    { id: '2', name: 'Ethereum', symbol: 'ETH', amount: 2, price: 3000 },
  ])
  
  const [newCrypto, setNewCrypto] = useState({
    name: '',
    symbol: '',
    amount: 0,
    price: 0
  })
  const [isAdding, setIsAdding] = useState(false)

  const handleAddCrypto = () => {
    if (!newCrypto.name || !newCrypto.symbol || newCrypto.amount <= 0 || newCrypto.price <= 0) {
      return
    }

    setCryptos([
      ...cryptos,
      {
        id: Date.now().toString(),
        name: newCrypto.name,
        symbol: newCrypto.symbol.toUpperCase(),
        amount: newCrypto.amount,
        price: newCrypto.price
      }
    ])
    
    setNewCrypto({ name: '', symbol: '', amount: 0, price: 0 })
    setIsAdding(false)
  }

  const handleRemoveCrypto = (id: string) => {
    setCryptos(cryptos.filter(crypto => crypto.id !== id))
  }

  const totalValue = cryptos.reduce((sum, crypto) => sum + (crypto.amount * crypto.price), 0)

  return (
    <Card className="w-full max-w-2xl mx-auto">
      <CardHeader>
        <CardTitle className="text-2xl font-bold">Crypto Portfolio</CardTitle>
      </CardHeader>
      <CardContent>
        <div className="mb-6">
          <div className="flex justify-between items-center mb-2">
            <h3 className="text-lg font-semibold">Total Portfolio Value</h3>
            <span className="text-xl font-bold">${totalValue.toLocaleString()}</span>
          </div>
        </div>

        <div className="space-y-4 mb-6">
          {cryptos.map(crypto => (
            <div key={crypto.id} className="flex items-center justify-between p-4 border rounded-lg">
              <div>
                <div className="font-bold">{crypto.name} ({crypto.symbol})</div>
                <div className="text-sm text-gray-500">
                  {crypto.amount} {crypto.symbol} @ ${crypto.price.toLocaleString()}
                </div>
              </div>
              <div className="text-right">
                <div className="font-bold">${(crypto.amount * crypto.price).toLocaleString()}</div>
                <Button 
                  variant="ghost" 
                  size="sm" 
                  className="text-red-500 hover:text-red-600"
                  onClick={() => handleRemoveCrypto(crypto.id)}
                >
                  <Trash className="w-4 h-4" />
                </Button>
              </div>
            </div>
          ))}
        </div>

        {isAdding ? (
          <div className="space-y-4">
            <div className="grid grid-cols-2 gap-4">
              <div>
                <Label htmlFor="name">Name</Label>
                <Input
                  id="name"
                  value={newCrypto.name}
                  onChange={(e) => setNewCrypto({...newCrypto, name: e.target.value})}
                  placeholder="Bitcoin"
                />
              </div>
              <div>
                <Label htmlFor="symbol">Symbol</Label>
                <Input
                  id="symbol"
                  value={newCrypto.symbol}
                  onChange={(e) => setNewCrypto({...newCrypto, symbol: e.target.value})}
                  placeholder="BTC"
                />
              </div>
            </div>
            <div className="grid grid-cols-2 gap-4">
              <div>
                <Label htmlFor="amount">Amount</Label>
                <Input
                  id="amount"
                  type="number"
                  value={newCrypto.amount || ''}
                  onChange={(e) => setNewCrypto({...newCrypto, amount: parseFloat(e.target.value) || 0})}
                  placeholder="0.5"
                />
              </div>
              <div>
                <Label htmlFor="price">Price per coin ($)</Label>
                <Input
                  id="price"
                  type="number"
                  value={newCrypto.price || ''}
                  onChange={(e) => setNewCrypto({...newCrypto, price: parseFloat(e.target.value) || 0})}
                  placeholder="50000"
                />
              </div>
            </div>
            <div className="flex space-x-2">
              <Button onClick={handleAddCrypto}>Add Crypto</Button>
              <Button variant="outline" onClick={() => setIsAdding(false)}>Cancel</Button>
            </div>
          </div>
        ) : (
          <Button onClick={() => setIsAdding(true)}>
            <Plus className="w-4 h-4 mr-2" />
            Add Cryptocurrency
          </Button>
        )}
      </CardContent>
    </Card>
  )
}
